## 1. Architecture Overview

- **Type**: Single‑page application (SPA) with serverless API backend, deployed entirely on Cloudflare infrastructure.
- **Deployment**: Next.js 15 (App Router) compiled with `@cloudflare/next-on-pages` to run on Cloudflare Pages (frontend + API routes).
- **Database**: Cloudflare D1 (SQLite‑compatible, serverless) for all persistent data – accounts, transactions, net‑worth snapshots, etc.
- **Scheduled tasks**: Cloudflare Workers Cron Triggers for periodic aggregation (daily balance pulls, monthly net‑worth snapshots).
- **Integrations**:
    - **Official APIs**: Monzo, Starling, Trading 212 – direct OAuth calls from Workers.
    - **Scrapers** (for banks without APIs): Optional microservice (see “Scraping Strategy”) that pushes data to the Worker API.
- **Manual fallback**: CSV/OFX import module for accounts that resist automation.

## 2. Tech Stack

|Layer|Choice|Reason|
|---|---|---|
|Frontend|Next.js 15 (App Router)|Familiar, powerful routing, SSR → ISR → static where possible|
|Styling|Tailwind CSS + shadcn/ui|Consistent design, pre‑built components, low bundle size|
|Charts|Recharts|Lightweight, React‑native, good for dashboards|
|Backend (API)|Next.js Route Handlers (deployed on Workers)|Same repo, no separate service, full‑stack on Workers|
|Database|Cloudflare D1|Relational, free tier sufficient, co‑located with Workers, fast reads|
|Cache / KV|Cloudflare KV (for OAuth tokens, short‑lived data)|Global, low‑latency, ideal for tokens|
|File storage|Cloudflare R2 (for import/export CSVs, receipts)|S3‑compatible, free tier 10 GB|
|Cron jobs|Cloudflare Workers Cron Triggers|Run scrapers, balance checks, snapshot creation|
|Scraper runtime|Local server / Raspberry Pi / Oracle Always Free VM + Puppeteer|Banks with heavy anti‑bot protection – headless browser required|
|Auth (optional)|Cloudflare Access / simple password stored as env var|Personal tool – minimal auth needed|

## 3. Database Schema (D1)

All tables live in a single D1 database.

```sql
-- Accounts (bank, investment, credit card, offline)
CREATE TABLE accounts (
  id TEXT PRIMARY KEY,                -- e.g. "starling_personal"
  name TEXT NOT NULL,
  type TEXT NOT NULL,                -- 'current', 'investment', 'credit_card', 'manual_asset', 'liability'
  provider TEXT,                     -- 'starling', 'monzo', 't212', 'offline', etc.
  integration_type TEXT,             -- 'api', 'scraper', 'manual'
  meta JSON                         -- extra: account_number (masked), sort_code, etc.
);

-- Transactions (every movement of money)
CREATE TABLE transactions (
  id TEXT PRIMARY KEY,               -- UUID
  account_id TEXT NOT NULL REFERENCES accounts(id),
  date TEXT NOT NULL,                 -- ISO 8601 date
  description TEXT,
  amount DECIMAL(10,2) NOT NULL,     -- positive = credit (income), negative = debit (expense)
  currency TEXT DEFAULT 'GBP',
  category TEXT,                     -- e.g. 'groceries', 'salary', 'investment_buy'
  tags JSON,                         -- ["subscription", "needs"]
  source TEXT,                       -- 'api', 'scraper', 'manual'
  is_pending BOOLEAN DEFAULT 0,
  FOREIGN KEY (account_id) REFERENCES accounts(id)
);

-- Net worth snapshots (recorded monthly)
CREATE TABLE net_worth_snapshots (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  date TEXT NOT NULL,                 -- e.g. '2025-07-31'
  total_assets DECIMAL(12,2),
  total_liabilities DECIMAL(12,2),
  net_worth DECIMAL(12,2),
  breakdown JSON                     -- account-by-account balances at that date
);

-- Subscriptions (detected recurring charges)
CREATE TABLE subscriptions (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  merchant TEXT NOT NULL,
  amount DECIMAL(8,2),
  last_charge_date TEXT,
  frequency TEXT,                    -- 'monthly', 'yearly'
  active BOOLEAN DEFAULT 1
);

-- Manual assets / liabilities (offline gold, car, student loan)
CREATE TABLE manual_assets (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  name TEXT NOT NULL,
  type TEXT NOT NULL,                -- 'asset' or 'liability'
  value DECIMAL(12,2),
  last_updated TEXT,
  notes TEXT
);
```

## 4. API Endpoints (Next.js Route Handlers)

All routes under `/api/`. Secured with a simple token check (`Authorization: Bearer <SECRET>` stored in environment variables).

### 4.1 Data Ingestion

- `POST /api/transactions`  
    Body: `{ account_id, transactions[] }`  
    Upserts transactions (idempotent by transaction ID). Called by API fetch workers, scrapers, or manual upload.
    
- `POST /api/balances`  
    Body: `{ account_id, balance, available, timestamp }`  
    Records current balance. Used for real‑time dashboard and net‑worth calculation.
    
- `POST /api/import/csv`  
    Multipart upload of bank‑exported CSV. Parse columns, map to transactions, store.
    

### 4.2 Querying & Analytics

- `GET /api/accounts` – list all accounts with latest balance.
- `GET /api/transactions?start=&end=&category=&account_id=&min_amount=&max_amount=`
- `GET /api/insights/spending?month=`  
    Returns: total spending, breakdown by category, top 5 transactions, comparison to previous month.
- `GET /api/insights/income?year=`  
    Returns: income sources (salary, side‑hustle, dividends, interest), aggregates, vs target.
- `GET /api/insights/subscriptions`  
    Returns list of detected active subscriptions (see Section 7.1).
- `GET /api/networth/history` – return all snapshots.
- `POST /api/networth/snapshot` – triggers a snapshot now (used manually or by cron).

### 4.3 Decision Engine

- `GET /api/recommendations`  
    Runs the decision logic (Section 8) and returns actionable items as a list of strings.

## 5. Integrations & Data Ingestion Flow

### 5.1 Official APIs (Monzo, Starling, Trading 212)

- Each API uses OAuth2 (or personal access tokens). Tokens stored encrypted in D1 (`accounts.meta`).
- **Scheduled fetcher worker**: A Cron‑triggered Worker calls each API daily:
    1. Retrieve tokens from DB.
    2. Fetch new transactions since last stored `update_id`.
    3. Push them to `POST /api/transactions`.
    4. Update `last_fetched` timestamp.
- Starling: `/api/v2/feed/items/{accountUid}` for transactions.
- Monzo: `https://api.monzo.com/transactions?expand[]=merchant&account_id=xxx`.
- Trading 212: `/api/v0/equity/account/cash` for balance, detailed portfolio via `/api/v0/equity/portfolio`.

### 5.2 Scrapers (Santander, Barclays, Mettle, Lloyd, Amex, Barclaycard, NS&I, InvestEngine)

- Due to heavy bot protection, scraping is done **off‑Cloudflare** using Puppeteer/Playwright on a low‑cost VM or a home Raspberry Pi.
- **Architecture**:
    - A Node.js script (scheduled via cron on the VM) logs into each bank portal using stored credentials (encrypted).
    - Scans recent transactions and balances, formats them, and `POST`s to the Cloudflare endpoint `/api/transactions`.
    - OTP handling: can be semi‑automated with a Telegram bot prompt if 2FA needed.
- **Alternative**: Manual upload via CSV – the system provides a clear import UI for each account. You can download statements and drag‑and‑drop them.

### 5.3 Manual Offline Assets

- UI under “Assets” lets you add/edit items (car, gold, cash). Value stored with a date, used in net‑worth snapshots.
- Car value can be semi‑automated by fetching from a car valuation API (e.g., Parker’s) inside a Worker.

## 6. Frontend Structure (Next.js Pages)

All pages under `/app/dashboard/` (protected by a simple login gate using `next-auth` with a credentials provider or just a password page).

- **Layout**: Sidebar navigation with sections: Overview, Accounts, Transactions, Net Worth, Recommendations, Settings.
- **Dashboard home**:
    - Summary cards: total assets, total liabilities, net worth.
    - Sparkline of net worth over last 12 months.
    - This month’s spending vs last month; top spending categories.
    - Quick list of recent transactions (last 5).
- **Spending page**:
    - Date range filter.
    - Pie/bar chart: spending by category (Needs / Wants / Investments).
    - List of largest transactions.
    - Subscription tracker – cards showing merchant, amount, next expected date, alert if spike.
- **Income page**:
    - Revenue mix chart (pie) + table.
    - Year‑on‑year income comparison.
    - Net cash flow (income – spending) and suggestion box.
- **Net Worth page**:
    - Line chart of historical net worth with simple linear trend projection.
    - Asset composition tree map.
    - Ratio monitor: (Investments + Savings) / Total Assets gauge.
- **Transactions page**:
    - Searchable, filterable table of all transactions with infinite scroll.
    - Ability to manually add/edit transactions.
- **Recommendations page**:
    - Cards displaying suggestions generated by the decision engine (Section 8), with checkboxes to mark as done/ignored.
- **Settings / Admin**:
    - API token management.
    - Category mapping (custom mapping of transaction descriptions to categories).
    - Define 6‑month expense threshold, student loan interest rate, target ratio.

## 7. Feature‑by‑Feature Implementation Details

### 7.1 Subscription Detection

- A scheduled Worker runs monthly: aggregates all transactions from the last 3 months, groups by `description` and approximate amount, flags groups with consistent frequency (e.g., 28–31 days, or same day each month).
- Results stored in `subscriptions` table; frontend displays them with a toggle to manually confirm.
- **Spike alert**: if a subscription amount exceeds its 3‑month rolling average by >20%, the system shows a warning on the dashboard.

### 7.2 Net Worth Snapshot Engine

- Cron Worker triggers on the 1st of each month:
    1. Fetch latest balances from all accounts (via stored data or, if API available, direct pull).
    2. Fetch manual asset valuations.
    3. Compute total assets, total liabilities, net worth.
    4. Insert into `net_worth_snapshots`.
- The history is plotted with a simple linear regression for projection (using the last 12 snapshots).

### 7.3 Decision‑Support Engine

The `GET /api/recommendations` endpoint runs this logic:

1. **Excess cash check**: Sum balances of all current accounts. Compare with 6 months of average expenses (pre‑calculated from transaction history). If cash > threshold * 1.2: “Move £X to high‑yield savings (InvestEngine/Trading212)”.
2. **Student loan optimisation**: If student loan interest rate (from manual input) > expected return of your favourite index fund (settable, default 7%), suggest overpaying by a specific monthly amount.
3. **Credit card utilisation**: If any credit card balance > 30% of its limit, warn about credit score impact.
4. **Subscription audit**: Flag any unused subscriptions (detected but not manually confirmed, or no recent transaction).
5. **SWR (Safe Withdrawal) check**: purely informational – show how much passive income your investments could generate.

Recommendations are returned as a list of objects `{ type, severity, message, action }`.

## 8. Security & Privacy

- **Environment variables**: All secrets (API keys, OAuth client secrets, internal API token) stored in Cloudflare Pages env vars, never in code.
- **D1 encryption**: Data is encrypted at rest.
- **HTTPS only**: All communications forced.
- **Access control**: The dashboard is behind a simple password gate (using `next-auth` with credentials or a Cloudflare Access policy). No external exposure.
- **Scraper credentials**: Kept on the scraping VM, transmitted only over HTTPS to the Worker API; the Worker never stores raw banking passwords.
- **GDPR / data residency**: Data stays in D1 (EU region selectable) – adequate for personal use.

## 9. Development & Deployment Workflow

- **Git repository**: Private GitHub/GitLab repo.
- **Monorepo structure**:
    
    ```
    /apps
      /web          # Next.js app (frontend + API routes)
      /workers
        /cron-fetcher   # Scheduled API integrations
        /cron-snapshot  # Net worth snapshot
        /cron-subscription # Subscription detection
    /packages
      /shared       # TS types, D1 ORM (Drizzle), helpers
    /scrapers
      /bank-scraper # Optional Puppeteer scripts
    ```
    
- **Local dev**: `wrangler dev` for Workers, `next dev` for web.
- **Deploy**: GitHub Actions → `wrangler pages deploy` for the Next.js app; separate deploy for Cron Workers using `wrangler deploy`.
- **Database migrations**: Drizzle Kit with D1, applied via `wrangler d1 execute`.

## 10. Cost Estimation

- Cloudflare Pages (free tier) – sufficient for personal use.
- Cloudflare Workers (free tier includes 100K requests/day, 10ms CPU per request) – well within limits.
- D1 (free tier: 5 GB storage, 5 million reads/month, 100K writes/month) – adequate.
- R2 (free tier: 10 GB storage, 10 million operations) – for CSV storage, never hit.
- Scraper VM: if using a Raspberry Pi at home, zero cost; otherwise a $5/month VPS (Hetzner) is an option.

**Total cost: €0–€5/month including optional VM.**