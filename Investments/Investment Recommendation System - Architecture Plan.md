# Investment Recommendation System — Architecture Plan

> **Status:** Planning | **Date:** 2026-05-27 | **Decisions made:** Both UK + US real estate ✓ | Portfolio tracking ✓ | Not starting yet

**Goal:** Multi-asset investment recommendation system covering stocks, crypto, and real estate (US + UK), with portfolio tracking and a web dashboard, delivered via Telegram.

**Architecture:** Hermes CronJobs on k3s → SQLite (market data + portfolio holdings) → LLM analysis engine (portfolio-aware) → FastAPI dashboard + Telegram digest.

**Tech Stack:** Hermes CronJobs, Python, SQLite, FastAPI + Jinja2 + Chart.js, yfinance, CoinGecko, FRED + Zillow ZHVI, HM Land Registry + ONS UK HPI.

---

## System Overview

```
┌──────────────────────────────────────────────────────────────┐
│                    SCHEDULER (Hermes Cron)                    │
│                                                              │
│  ┌──────────────┐  ┌──────────────┐  ┌───────────────┐      │
│  │ Data Collect │  │ Data Collect │  │ Data Collect  │      │
│  │   Stocks     │  │    Crypto    │  │  Real Estate  │      │
│  │  (daily)     │  │  (daily)     │  │  (weekly)     │      │
│  │              │  │              │  │ US + UK       │      │
│  └──────┬───────┘  └──────┬───────┘  └───────┬───────┘      │
│         │                 │                   │              │
│         └────────┬────────┴───────────────────┘              │
│                  ▼                                           │
│  ┌───────────────────────────────┐                           │
│  │         SQLite DB             │ ← market data              │
│  │     /opt/data/market_data.db  │    + recommendations      │
│  │                               │    + portfolio holdings    │
│  └────────┬──────────────┬───────┘                           │
│           │              │                                    │
│           ▼              ▼                                    │
│  ┌───────────────┐  ┌──────────────────────┐                 │
│  │  Analysis     │  │  Dashboard (FastAPI)  │ ← READ-ONLY    │
│  │  Engine       │  │  Port 8080, k3s pod   │   web view     │
│  │  (weekly)     │  │  + portfolio page     │                │
│  └───────┬───────┘  └──────────────────────┘                 │
│          │                                                    │
│  ┌───────▼───────┐                                           │
│  │  Digest       │ → Telegram (+ Portfolio Health section)   │
│  │  Compiler     │ → Obsidian archival                       │
│  │  (weekly)     │                                           │
│  └───────────────┘                                           │
└──────────────────────────────────────────────────────────────┘
```

---

## Component 1: Data Storage

### `market_data` table
- `symbol`, `asset_class` (stock/crypto/real_estate), `metric`, `value`, `timestamp`, `metadata` (JSON)

### `recommendations` table
- `asset`, `action` (buy/sell/hold/watch/accumulate), `confidence`, `reasoning`, `signals` (JSON), `risk_level`, `horizon_days`

### `portfolio_holdings` table
- `asset`, `asset_class`, `quantity`, `avg_cost_basis`, `currency`, `acquired_at`, `notes`, `updated_at`
- **Populated:** Manually via dashboard form (v1), broker API (Phase 2+)
- **Feeds:** Allocation drift analysis, opportunity scoring, diversification recommendations

**Storage:** SQLite at `/opt/data/market_data.db`, WAL mode.

---

## Component 2: Data Collectors

### Stocks (Daily)
- **Source:** `yfinance` (free)
- **Data:** OHLC price, volume, market cap, P/E, 50/200 MA, RSI

### Crypto (Daily)
- **Source:** CoinGecko free API
- **Data:** Price, volume, market cap, 7d/30d change %, ATH/ATL

### Real Estate (Weekly) — US + UK

**US Pipeline:**
- FRED API: mortgage rates, housing starts, Case-Shiller index
- Zillow Research CSV: ZHVI by metro area

**UK Pipeline:**
- HM Land Registry: Price Paid Data CSV (every sale, ~2mo lag)
- ONS UK HPI: Official index by region
- Bank of England: mortgage rates, approvals

```yaml
real_estate:
  us:
    metros: ["New York, NY", "Austin, TX", "Miami, FL"]
  uk:
    regions: ["London", "South East", "North West", "Scotland"]
    cities: ["Manchester", "Birmingham", "Edinburgh"]
```

**Note:** UK data is laggy. Analysis engine uses trend-based signals for UK.

### News/Sentiment (Phase 2)
- NewsAPI or RSS feeds — deferred

---

## Component 3: Analysis Engine (Weekly)

LLM-powered, portfolio-aware. Runs Sunday evening:

1. Reads last 1-4 weeks of market data + portfolio holdings
2. Computes quantitative signals (RSI, MAs, volume, drawdown, WoW/MoM %)
3. Feeds into LLM with structured prompt (conservative, evidence-based, no hype)
4. Outputs JSON recommendations → `recommendations` table
5. Portfolio-aware outputs: allocation drift, diversification gaps, "what to buy next"

---

## Component 4: Digest & Delivery (Weekly)

Markdown digest with sections:
- Market Overview
- Top Recommendations (stocks, crypto, real estate — US + UK)
- Risk Alerts
- **Portfolio Health Check** — allocation, P&L, drift alerts, diversification gaps

**Delivery:** Telegram (primary) + Obsidian archival (`Investments/Digests/`)

---

## Component 5: Web Dashboard

**Stack:** FastAPI + Jinja2 + Chart.js (no frontend build step)

| Route | Content |
|-------|---------|
| `/` | Overview cards, latest recommendations |
| `/portfolio` | Holdings, live P&L, allocation pie, drift alerts, edit form |
| `/stocks` | Watchlist with sparklines, RSI |
| `/crypto` | 7d/30d trends, ATH drawdown |
| `/real-estate` | US + UK tabs, ZHVI trends, mortgage rates |
| `/asset/<symbol>` | Price history chart, technicals, rec history |
| `/recommendations` | Full history, filterable |

**Deployment:** k3s pod mounting Hermes PVC (read-only). Internal access or Cloudflare Access.

---

## Component 6: Configuration

```yaml
stocks:
  watchlist: [AAPL, MSFT, GOOGL, NVDA, TSLA, AMZN, META, BRK-B]
  indices: [^GSPC, ^IXIC, ^DJI]

crypto:
  watchlist: [bitcoin, ethereum, solana, cardano, polkadot]

real_estate:
  us:
    metros: ["New York, NY", "Austin, TX", "Miami, FL"]
  uk:
    regions: ["London", "South East", "North West", "Scotland"]
    cities: ["Manchester", "Birmingham", "Edinburgh"]

analysis:
  horizon_days: 30
  min_confidence: low
  risk_tolerance: moderate

portfolio:
  target_allocation:
    stocks: 60
    crypto: 20
    cash: 10
    real_estate: 10
  drift_threshold_pct: 5
  base_currency: GBP

delivery:
  telegram: true
  obsidian: true
  obsidian_path: "Investments/Digests"

alerts:  # future
  price_drop_pct: 15
  rsi_oversold: 30
  rsi_overbought: 70
```

---

## Implementation Phases

### Phase 1: Foundation
- [ ] SQLite schema (market_data + recommendations + portfolio_holdings) + Python DB module
- [ ] `investment_config.yaml` with watchlists, target allocation, UK + US regions
- [ ] Stock data collector CronJob (daily)
- [ ] Verify data collection

### Phase 2: Crypto + Real Estate
- [ ] Crypto data collector CronJob (daily)
- [ ] Real estate data collector CronJob (weekly) — both US and UK pipelines
- [ ] Verify all pipelines

### Phase 3: Dashboard
- [ ] FastAPI app + Jinja2 templates
- [ ] Overview + Portfolio (holdings, allocation pie, P&L, edit form) + Stock detail + Crypto + Real Estate (US/UK tabs)
- [ ] Chart.js price history
- [ ] Dockerfile + k3s manifests, deploy

### Phase 4: Analysis Engine
- [ ] Quantitative signals + LLM prompt (portfolio-aware)
- [ ] Analysis CronJob → recommendations table
- [ ] Dashboard shows recommendations automatically

### Phase 5: Digest & Delivery
- [ ] Markdown digest with Portfolio Health Check
- [ ] Telegram delivery + Obsidian archival
- [ ] End-to-end test

### Phase 6: Polish
- [ ] Dark theme + mobile responsive
- [ ] News/sentiment integration
- [ ] Price drop alerts → Telegram
- [ ] Broker API for auto-importing holdings
- [ ] Recommendation performance tracking

---

## Decisions Made

- ✅ **UK + US real estate** — both regions. US: FRED + Zillow. UK: Land Registry + ONS HPI + BoE.
- ✅ **Portfolio tracking** — both watchlist AND actual holdings. `portfolio_holdings` table + dashboard form.
- ⏸️ **Not starting yet** — plan is locked, implementation deferred.

## Still Open

- **CronJob chaining:** Fixed schedule or auto-trigger after collectors?
- **Backtesting:** Track recommendation accuracy?

---

## Risks

| Risk | Mitigation |
|------|------------|
| yfinance API breaks | Community-maintained; fallback to Alpha Vantage |
| CoinGecko rate limits | 2s delay between calls; CoinMarketCap fallback |
| UK data is laggy (2mo) | Analysis engine uses trend-based signals for UK real estate |
| LLM hallucinates financial advice | Prompt rules: cite data, flag uncertainty, mandatory disclaimer |
| CronJob failures | Telegram failure alerts |
| SQLite corruption | Periodic R2 backups |
| SQLite lock contention | WAL mode; cron-scheduled writes |
| Dashboard can't reach DB | Same PVC mount |
| Dashboard exposed | Internal-only by default; Cloudflare Access if external |

---

## ⚠️ Disclaimer

This is an automated system powered by AI. **Not financial advice.** Do your own research before making investment decisions. Past performance ≠ future results.

---

*Linked from: [[Current AI Trends - May 2026]]*
*Hermes plan: `.hermes/plans/2026-05-27_000000-investment-recommendation-system.md`*
