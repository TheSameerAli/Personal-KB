# Investment Recommendation System — Architecture Plan

> **Status:** Planning — all decisions locked | **Date:** 2026-05-27 | **Not starting yet**

**Goal:** Multi-asset investment recommendation system covering stocks, crypto, and real estate (US + UK), with portfolio tracking, backtesting, and a web dashboard, delivered via Telegram.

**Architecture:** Hermes CronJobs on k3s → SQLite (market data + portfolio holdings + recommendations + backtest results) → LLM analysis engine (portfolio-aware, preflight-checked) → FastAPI dashboard + Telegram digest.

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
│  │                               │    + backtest results      │
│  └────────┬──────────────┬───────┘                           │
│           │              │                                    │
│           ▼              ▼                                    │
│  ┌───────────────┐  ┌──────────────────────┐                 │
│  │  Analysis     │  │  Dashboard (FastAPI)  │ ← READ-ONLY    │
│  │  Engine       │  │  Port 8080, k3s pod   │   web view     │
│  │  + preflight  │  │  + portfolio page     │                │
│  │  (weekly)     │  │  + accuracy card      │                │
│  └───────┬───────┘  └──────────────────────┘                 │
│          │                                                    │
│  ┌───────▼───────┐                                           │
│  │  Digest       │ → Telegram (+ Portfolio Health section)   │
│  │  Compiler     │ → Obsidian archival                       │
│  │  (weekly)     │                                           │
│  └───────────────┘                                           │
│                                                              │
│  ⏰ Phase 6: ┌───────────────┐                                │
│              │  Backtesting  │ ← evaluates expired recs       │
│              │  Engine       │   accuracy → dashboard card    │
│              │  (weekly)     │                                │
│              └───────────────┘                                │
└──────────────────────────────────────────────────────────────┘
```

---

## Component 1: Data Storage

### `market_data` — time-series price/volume/indicator data
### `recommendations` — LLM-generated buy/sell/hold/watch signals
### `portfolio_holdings` — user's actual positions (manual entry v1, broker API v2)
### `backtest_results` — recommendation accuracy tracking (Phase 6)

All in SQLite at `/opt/data/market_data.db`, WAL mode.

---

## Component 2: Data Collectors

### Stocks (Daily, `yfinance`)
OHLC price, volume, market cap, P/E, 50/200 MA, RSI

### Crypto (Daily, CoinGecko)
Price, volume, market cap, 7d/30d %, ATH/ATL, supply

### Real Estate (Weekly) — US + UK

**US:** FRED API (mortgage rates, housing starts, Case-Shiller) + Zillow ZHVI CSV
**UK:** HM Land Registry Price Paid CSV + ONS UK HPI + Bank of England rates
**Note:** UK data ~2mo laggy → analysis uses trend-based signals

---

## Component 3: Analysis Engine (Weekly, Sunday 6pm)

**Preflight check** before running:
- Stocks fresh within 24h (72h on Monday, covering Friday)
- Crypto fresh within 24h
- Real estate fresh within 7 days
- If stale → `⚠️` warning in digest + Telegram alert, but still runs

**Analysis:** Reads market data + portfolio holdings → computes quantitative signals → LLM produces portfolio-aware recommendations → writes to `recommendations` table.

---

## Component 4: Digest & Delivery (Weekly)

Markdown digest: Market Overview · Top Recommendations · Risk Alerts · **Portfolio Health Check** (P&L, drift, diversification gaps)

**Delivery:** Telegram + Obsidian archival

---

## Component 5: Web Dashboard

**Stack:** FastAPI + Jinja2 + Chart.js (no frontend build)

| Route | Content |
|-------|---------|
| `/` | Overview cards, latest recs |
| `/portfolio` | Holdings, live P&L, allocation pie, drift, edit form |
| `/stocks` | Watchlist, sparklines, RSI |
| `/crypto` | 7d/30d trends, ATH drawdown |
| `/real-estate` | US + UK tabs, ZHVI, mortgage rates |
| `/asset/<symbol>` | Price chart, technicals, rec history |
| `/recommendations` | Full history, filterable |

**Accuracy card (Phase 6):** Overall hit rate, by asset class, by confidence level

**Deployment:** k3s pod, PVC mount (read-only), internal or Cloudflare Access

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

## Component 7: Backtesting Engine (Phase 6)

- Recommendations have 30-day horizon → expired recs checked against actual price
- **Only scores explicit buy/sell calls** (holds/watches noted but not scored)
- Results → `backtest_results` table → accuracy % on dashboard
- If accuracy <50% over 90 days → "prompt review suggested" warning
- Designed now (schema supports from Phase 1), engine built in Phase 6

---

## Implementation Phases

### Phase 1: Foundation
- SQLite schema (all 4 tables) + Python DB module
- `investment_config.yaml`
- Stock data collector CronJob

### Phase 2: Crypto + Real Estate
- Crypto + Real Estate (US + UK) collectors

### Phase 3: Dashboard
- FastAPI + Jinja2 + Chart.js, all pages, k3s deploy

### Phase 4: Analysis Engine
- Quantitative signals + LLM prompt + preflight check
- Analysis CronJob → recommendations table

### Phase 5: Digest & Delivery
- Markdown digest + Portfolio Health Check
- Telegram delivery + Obsidian archival

### Phase 6: Polish
- Dark theme, mobile, news/sentiment, price alerts
- Backtesting engine + accuracy dashboard card
- Broker API, prompt tuning

---

## Decisions — All Locked

| # | Question | Decision |
|---|----------|----------|
| 1 | Portfolio tracking? | ✅ Both watchlist + actual holdings |
| 2 | UK real estate? | ✅ Both US + UK pipelines |
| 3 | CronJob chaining? | ✅ Fixed schedule + preflight freshness check with stale-data warnings |
| 4 | Backtesting? | ✅ Lightweight, buy/sell only, Phase 6, schema supports from day 1 |
| 5 | Broker API? | ⏸️ Phase 2+, manual entry for v1 |

---

## Risks

| Risk | Mitigation |
|------|------------|
| yfinance breaks | Community-maintained; Alpha Vantage fallback |
| CoinGecko rate limits | 2s delay; CoinMarketCap fallback |
| UK data laggy | Trend-based signals for UK real estate |
| LLM hallucinates | Cite data, flag uncertainty, disclaimer |
| CronJob failures | Telegram alerts |
| SQLite corruption | R2 backups |
| Lock contention | WAL mode; cron-scheduled writes |
| Dashboard exposed | Internal-only; Cloudflare Access if needed |

---

## ⚠️ Disclaimer

This is an automated system powered by AI. **Not financial advice.** Do your own research. Past performance ≠ future results.

---

*Linked from: [[Current AI Trends - May 2026]]*
