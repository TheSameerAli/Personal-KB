Related: [[Personal Finance System]] [[PFS - Technical Plan]]
# Personal Finance System – Master Prompt PRD

> **You are a senior full‑stack engineer and architect.**  
> Build a complete, production‑ready personal finance system following this document exactly.  
> Use Next.js 15 (App Router), Tailwind CSS, shadcn/ui, Recharts for charts, Cloudflare D1 as database, Cloudflare Workers for cron, and Cloudflare Pages for deployment.  
> Fill in all code, environment variables, and minor implementation details that are not explicitly specified.  
> The system must be fully self‑contained, secure, and follow the architecture below.

---

## 1. Project Overview

A personal finance dashboard that:

- Ingests transactions from **multiple bank / investment / credit card accounts** (some via official APIs, others via scrapers or manual CSV import).
- Tracks **manual offline assets** (gold, cash, car, student loan).
- Provides:
  - **Spending Overview** with categories, trends, alerts, subscription detection.
  - **Income Tracker** with source breakdown and net cash flow.
  - **Net Worth** calculation, monthly snapshots, and projections.
  - **Decision‑Support Engine** that generates actionable recommendations.
- Runs entirely on Cloudflare free tier (Zero‑cost operation).

---

## 2. Accounts & Data Sources

The user has these accounts. The system must support all of them:

| Account | Type | Integration | Details |
|---------|------|--------------|---------|
| Santander | Current | Scraper / Manual import | |
| Barclays | Current | Scraper / Manual import | |
| Monzo | Current | Official API | [[Monzo API Reference]] – OAuth |
| Starling | Current | Official API | [[Starling API Documentation]] |
| Mettle | Current | Scraper (Natwest Open Banking if possible, else scraper) | |
| Trading 212 | Investment | Official API | [[Trading 212 Public API]] |
| InvestEngine | Investment | Scraper / Manual | |
| NS&I | Bonds | Scraper / Manual | |
| Lloyds Credit Card | Credit Card | Scraper / Manual | Limit £4,500 |
| Amex | Credit Card | Scraper / Manual | Limit £9,990 |
| Barclaycard | Credit Card | Scraper / Manual | Limit £1,200 |
| Car | Manual Asset | Manual | Value entered manually, date stamped |
| Gold / Cash (offline vault) | Manual Asset | Manual | Value entered manually, date stamped |

- **All accounts** are represented in the `accounts` table.
- For API‑connected accounts, a scheduled worker fetches transactions daily.
- For scraper accounts, a separate script (not on Cloudflare) pushes data to a secure endpoint.
- For manual assets, the front end provides a form to add/edit value and date.

---

## 3. Features – Detailed Requirements

### 3.1 Spending Overview

- Dashboard page “Spending” shows:
  - Total spending for selected month / year.
  - Spending breakdown by **Needs / Wants / Investments** (categories mapped via a configurable rule set).
  - Month‑over‑month and year‑over‑year trends (line or bar chart).
  - Top 5 largest transactions for the period (to catch one‑off regrets).
- **Threshold alerts**: Define per‑category rules (e.g., “Phone bill > £60”) – if breached, show a warning on the dashboard.
- **Subscription detection**:
  - Automatic: find merchants that appear regularly (group by description + amount similarity) and surface as active subscriptions.
  - Show list of subscriptions with last date, amount, and frequency.
  - **Spike detection**: if a recurring amount jumps >20% from its 3‑month average, alert the user.

### 3.2 Income Tracker

- Detect income transactions (positive amounts) tagged by source: Salary, Side‑Hustle, Dividends, Interest.
- **Revenue mix** chart (pie chart) for a selected year.
- Set **income targets** (e.g., “£3,000/month total”) and show progress.
- Display **Net Cash Flow** = total income – total spending for the month.
- If net cash flow positive, suggest **excess cash** that could be moved to investments (ties into recommendation engine).

### 3.3 Net Worth Calculation & Tracking

- **Asset aggregation**:
  - Live balances from bank/investment accounts (via API/scraper).
  - Manual assets: `manual_assets` table with name, type (asset/liability), value, last_updated.
- **Monthly snapshots**:
  - A cron worker on the 1st of each month captures all balances and manual asset values, computes total assets, total liabilities, net worth, and stores in `net_worth_snapshots`.
- **Net worth history page**:
  - Line chart of net worth over time.
  - Simple linear regression projection (next 6 months) based on last 12 snapshots.
  - Highlight major changes (e.g., a bonus or market jump) via annotations if possible.
- **Investment ratio monitor**:
  - Ratio = (Investments + Savings) / Total Assets
  - Goal >20% (configurable). Show gauge on dashboard; if below, add recommendation.

### 3.4 Decision‑Support Engine

Run after every snapshot (or on demand) and return a list of recommendations:

1. **Excess cash check**:
   - Sum of all current account balances > 6 months of average expenses (configurable). Suggest moving surplus to InvestEngine / Trading 212 / NS&I.
   - Example: “You have £4,200 in current accounts — Move £2,000 to InvestEngine to earn higher returns.”
2. **Student loan optimisation**:
   - If student loan interest rate (manual input) > expected investment return (default 7%), suggest overpaying by a calculated monthly amount.
   - “Your student loan costs 6.3%. Overpaying £150/month could save £X in interest.”
3. **Credit card utilisation**:
   - For each credit card, if balance > 30% of limit, warn about credit score impact.
4. **Subscription audit**:
   - Flag subscriptions not marked as “active” (or no transaction in last 45 days) and suggest cancelling.
5. **SWR (informational)**:
   - Show current passive income potential: (Total Investments × 4%) / 12 per month.

Each recommendation is an object: `{ type, severity, message, action }`.

---

## 4. Architecture

- **Frontend**: Next.js 15 App Router, Tailwind CSS, shadcn/ui components, Recharts for visualisations.
- **Backend**: Next.js Route Handlers (API routes) deployed on Cloudflare Pages via `@cloudflare/next-on-pages`. No separate server.
- **Database**: Cloudflare D1 (SQLite‑compatible, serverless).
- **Scheduled tasks**: Cloudflare Workers Cron Triggers (3 separate workers):
  - `cron-fetcher` – daily API pulls.
  - `cron-snapshot` – monthly net worth snapshot.
  - `cron-subscription` – monthly subscription detection.
- **Scrapers**: Optional external Node.js script (Puppeteer) that pushes data to `/api/transactions`. Not part of the main repo, but the system must accept data from it.
- **File storage**: Cloudflare R2 for CSV imports (optional).
- **Auth**: Simple password gate – store a `SITE_PASSWORD` env var. All API routes check `Authorization: Bearer <SITE_PASSWORD>`. Frontend uses a login page.

---

## 5. Database Schema (D1)

```sql
-- Accounts
CREATE TABLE accounts (
  id TEXT PRIMARY KEY,
  name TEXT NOT NULL,
  type TEXT NOT NULL,                -- 'current', 'investment', 'credit_card', 'manual_asset', 'liability'
  provider TEXT,
  integration_type TEXT,             -- 'api', 'scraper', 'manual'
  meta JSON                          -- e.g., {"limit":4500} for credit card, API tokens for API accounts
);

-- Transactions
CREATE TABLE transactions (
  id TEXT PRIMARY KEY,
  account_id TEXT NOT NULL REFERENCES accounts(id),
  date TEXT NOT NULL,
  description TEXT,
  amount DECIMAL(10,2) NOT NULL,     -- positive = credit, negative = debit
  currency TEXT DEFAULT 'GBP',
  category TEXT,
  tags JSON,                         -- ["subscription", "needs"]
  source TEXT,                       -- 'api', 'scraper', 'manual'
  is_pending BOOLEAN DEFAULT 0
);

-- Net worth snapshots
CREATE TABLE net_worth_snapshots (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  date TEXT NOT NULL,                -- e.g., '2025-07-31'
  total_assets DECIMAL(12,2),
  total_liabilities DECIMAL(12,2),
  net_worth DECIMAL(12,2),
  breakdown JSON                     -- account-by-account balances at that date
);

-- Subscriptions (detected)
CREATE TABLE subscriptions (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  merchant TEXT NOT NULL,
  amount DECIMAL(8,2),
  last_charge_date TEXT,
  frequency TEXT,                    -- 'monthly', 'yearly'
  active BOOLEAN DEFAULT 1
);

-- Manual assets / liabilities
CREATE TABLE manual_assets (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  name TEXT NOT NULL,
  type TEXT NOT NULL,                -- 'asset' or 'liability'
  value DECIMAL(12,2),
  last_updated TEXT,
  notes TEXT
);

-- Settings (optional but useful)
CREATE TABLE settings (
  key TEXT PRIMARY KEY,
  value TEXT
);
```

---

## 6. API Endpoints

All routes under `/api/`. Protected with `Authorization: Bearer <SITE_PASSWORD>`.

### 6.1 Data Ingestion

- `POST /api/transactions`  
  Body: `{ account_id: string; transactions: { id, date, description, amount, currency, category?, tags?, is_pending? }[] }`  
  Upsert transactions by id.

- `POST /api/balances`  
  Body: `{ account_id, balance, available?, timestamp }`  
  Update the cached balance in a separate table or a view. (Simplified: store latest balance in `accounts.meta.balance`.)

- `POST /api/import/csv`  
  Accepts multipart upload. Parse CSV, map columns (date, description, amount), and call the same ingest logic.

### 6.2 Query & Analytics

- `GET /api/accounts` – all accounts with current balance (from latest balance update).
- `GET /api/transactions?start=&end=&category=&account_id=&min_amount=&max_amount=&page=&limit=`
- `GET /api/insights/spending?month=YYYY-MM`  
  Returns: total_spending, breakdown_by_category, top5_transactions, previous_month_total, alerts_triggered.
- `GET /api/insights/income?year=YYYY`  
  Returns: total_income, breakdown_by_source, target_comparison, net_cash_flow.
- `GET /api/insights/subscriptions` – all detected subscriptions with active flag.
- `GET /api/networth/history` – all snapshots.
- `POST /api/networth/snapshot` – triggers an immediate snapshot (manual or cron).
- `GET /api/recommendations` – runs decision engine and returns list.

### 6.3 Settings & Admin

- `PUT /api/settings` – update category mappings, thresholds, student loan rate, etc.
- `GET /api/settings` – retrieve all settings.

---

## 7. Frontend Pages & Components

### Layout

- Sidebar navigation: Overview, Spending, Income, Net Worth, Transactions, Recommendations, Settings.
- Mobile‑responsive drawer.
- Dark mode support (optional but nice).

### 7.1 Dashboard Home (`/`)

- Summary cards: Total Assets, Total Liabilities, Net Worth.
- Mini sparkline of net worth over last 12 months.
- This month’s spending vs last month with a percentage change indicator.
- Top 5 spending categories (horizontal bar).
- List of last 5 transactions.

### 7.2 Spending Page (`/spending`)

- Date range selector (month year).
- Pie chart: category breakdown (Needs/Wants/Investments).
- Table of largest transactions.
- Subscription tracker: cards with merchant, amount, next expected date, alert if spike.
- Threshold alert list.

### 7.3 Income Page (`/income`)

- Revenue mix chart (pie) + table.
- Year‑on‑year income comparison (bar chart).
- Net cash flow big number.
- Suggestion box for excess cash.

### 7.4 Net Worth Page (`/networth`)

- Line chart of historical net worth with projection.
- Tree map or simple list of asset composition.
- Gauge chart for investment ratio.
- Manual asset management (add/edit list).

### 7.5 Transactions Page (`/transactions`)

- Full searchable, filterable table of all transactions (by date, account, category, amount).
- Ability to manually add a transaction.
- Export to CSV button.

### 7.6 Recommendations Page (`/recommendations`)

- Cards for each recommendation with severity badge.
- Mark as done/ignore with a checkbox that persists (in a local state or a database flag).

### 7.7 Settings Page (`/settings`)

- Define category mappings (e.g., merchant contains “TESCO” → groceries).
- Set monthly expenses average, student loan rate, investment return expectation, target investment ratio.
- Configure API tokens for Monzo/Starling/T212 (if not done via env).

---

## 8. Scheduled Workers (Cron)

### 8.1 Daily Fetcher (`cron-fetcher`)

- Runs daily at 03:00 UTC.
- For each account marked `integration_type='api'`:
  - Monzo: fetch transactions since last stored `update_id`.
  - Starling: similar.
  - Trading 212: get portfolio cash balance, list of instruments, record as manual transactions? (simplified: treat investments as an account with balance, and record buy/sell as transactions from a “Trading 212” account)
- Push via `POST /api/transactions` with internal token.

### 8.2 Monthly Snapshot (`cron-snapshot`)

- Runs on the 1st of each month.
- Gathers:
  - Latest balance from each account (from stored balance or direct API call).
  - Values of all manual assets.
- Computes `total_assets`, `total_liabilities`, `net_worth`.
- Inserts into `net_worth_snapshots`.
- Triggers the recommendation engine and stores recommendations? (Optional – can be done on‑the‑fly).

### 8.3 Subscription Detection (`cron-subscription`)

- Runs on the 1st of each month.
- Query all transactions from last 3 months.
- Group by `description` and amount (rounded to nearest whole pound, then cluster amounts within 5%).
- For groups with >= 2 occurrences and timestamps that follow a regular monthly interval, insert/update `subscriptions`.
- Mark any previous subscription that didn't have a new charge as `active = 0`.
- Also detect spikes: compare each active sub’s latest amount with 3‑month average; if >20% increase, set an alert (maybe store in a separate table or just return via API).

---

## 9. Decision Engine Logic (Pseudo‑code for API)

```
function getRecommendations()
  settings = getSettings()
  currentAccounts = getAccountBalances()
  expenses6m = settings.monthly_avg_expense * 6
  totalCash = sum(currentAccounts.where(type='current').balance)

  recs = []

  if totalCash > expenses6m * 1.2
    excess = totalCash - expenses6m
    recs.push({
      type: 'cash_surplus',
      severity: 'medium',
      message: `You have £${excess.toFixed(0)} more than 6 months of expenses. Consider moving to InvestEngine/Trading 212.`
    })

  studentLoanRate = settings.student_loan_rate
  if studentLoanRate > settings.expected_investment_return
    recs.push({
      type: 'student_loan',
      severity: 'high',
      message: `Your student loan interest (${studentLoanRate}%) exceeds expected investment return. Overpaying may be beneficial.`
    })

  // credit card utilisation
  creditCards = currentAccounts.where(type='credit_card')
  for each cc in creditCards:
    if cc.balance / cc.limit > 0.3
      recs.push({type:'credit_use', severity:'high', message:`${cc.name} utilisation at ${(cc.balance/cc.limit*100).toFixed(0)}%. Consider paying down.`})

  // inactive subscriptions
  inactiveSubs = getSubscriptions().where(active == false or last_charge_date < 45 days ago)
  if inactiveSubs.length > 0
    recs.push({type:'sub_audit', severity:'low', message:'You have unused subscriptions. Review them on the Spending page.'})

  // SWR
  investTotal = sum(currentAccounts.where(type='investment').balance)
  swrMonthly = (investTotal * 0.04) / 12
  recs.push({type:'swr', severity:'info', message:`Your investments could safely generate ~£${swrMonthly.toFixed(2)}/month (4% rule).`})

  return recs
```

---

## 10. Security

- All secrets stored as Cloudflare Pages environment variables (`SITE_PASSWORD`, `MONZO_CLIENT_ID`, etc.).
- API routes check `Authorization` header.
- Frontend: a simple password prompt; on success, store token in sessionStorage and send with every request.
- No raw bank credentials stored in Cloudflare. The scraper runs externally and only pushes transaction data.
- HTTPS enforced.
- D1 data encrypted at rest.

---

## 11. Initial Setup & Configuration

The system must include a seed script or a UI onboarding flow:

1. User creates accounts entries (with proper ids).
2. Enters manual assets (car value, gold value).
3. Configures category mappings.
4. Sets thresholds and targets.

All of this is stored in D1.

---

## 12. Deployment

- **Frontend + API**: Deploy via `wrangler pages deploy` (Next.js with `@cloudflare/next-on-pages`).
- **Cron Workers**: Separate worker scripts deployed using `wrangler deploy`.
- **Database**: `wrangler d1 create` and migrations applied via Drizzle Kit.
- Final application is accessible behind Cloudflare Access or simple login page.

---

**Now, please build the entire application codebase, respecting all the above requirements, schemas, and architectural decisions.**