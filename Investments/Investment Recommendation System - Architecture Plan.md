# Investment Recommendation System — Architecture Plan

> **Status:** Planning | **Date:** 2026-05-27 | **Hermes Plan:** `.hermes/plans/2026-05-27_000000-investment-recommendation-system.md`

**Goal:** Multi-asset investment recommendation system producing scheduled digests (stocks, crypto, real estate) with a web dashboard, delivered via Telegram.

**Architecture:** Hermes CronJobs on the k3s homelab → SQLite DB → LLM-powered analysis → FastAPI dashboard + Telegram digest.

**Tech Stack:** Hermes CronJobs, Python scripts, SQLite, FastAPI + Jinja2 + Chart.js, yfinance, CoinGecko, FRED/Zillow CSV.

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
│  └──────┬───────┘  └──────┬───────┘  └───────┬───────┘      │
│         │                 │                   │              │
│         └────────┬────────┴───────────────────┘              │
│                  ▼                                           │
│  ┌───────────────────────────────┐                           │
│  │         SQLite DB             │ ← time-series market data  │
│  │     /opt/data/market_data.db  │    + recommendations      │
│  └────────┬──────────────┬───────┘                           │
│           │              │                                    │
│           ▼              ▼                                    │
│  ┌───────────────┐  ┌──────────────────────┐                 │
│  │  Analysis     │  │  Dashboard (FastAPI)  │ ← READ-ONLY    │
│  │  Engine       │  │  Port 8080, k3s pod   │   web view     │
│  │  (weekly)     │  │  Chart.js charts      │                │
│  └───────┬───────┘  └──────────────────────┘                 │
│          │                                                    │
│  ┌───────▼───────┐                                           │
│  │  Digest       │ ← Markdown report → Telegram + Obsidian   │
│  │  Compiler     │                                           │
│  │  (weekly)     │                                           │
│  └───────────────┘                                           │
└──────────────────────────────────────────────────────────────┘
```

**Data flow:** Collectors write to SQLite → Analysis engine reads DB, writes recommendations → Dashboard reads DB (read-only) → Digest Compiler reads DB, sends to Telegram.

---

## Component 1: Data Storage

### Schema — `market_data`
- `symbol` (TEXT) — ticker, token ID, or metro name
- `asset_class` (TEXT) — `stock` | `crypto` | `real_estate`
- `metric` (TEXT) — `price` | `volume` | `market_cap` | `zhvi` | etc.
- `value` (REAL)
- `timestamp` (TEXT) — ISO 8601
- `metadata` (JSON) — name, sector, chain, location, etc.

### Schema — `recommendations`
- `id` (INTEGER PRIMARY KEY)
- `generated_at` (TEXT)
- `asset` (TEXT)
- `asset_class` (TEXT)
- `action` (TEXT) — `buy` | `sell` | `hold` | `watch` | `accumulate`
- `confidence` (TEXT) — `low` | `medium` | `high`
- `reasoning` (TEXT)
- `signals` (JSON)
- `risk_level` (TEXT)
- `horizon_days` (INTEGER)

**Storage:** SQLite at `/opt/data/market_data.db`. WAL mode for concurrent reads (dashboard) + writes (collectors/analysis).

---

## Component 2: Data Collectors

### 2a. Stocks (Daily, after market close)
- **Source:** `yfinance` (free, no API key)
- **Data:** Price (OHLC), volume, market cap, P/E, 50/200 MA, RSI
- **Config:** `investment_config.yaml` — user-curated watchlist + indices (S&P 500, NASDAQ, DJI)

### 2b. Crypto (Daily, mornings)
- **Source:** CoinGecko free API (`/api/v3/coins/markets`)
- **Data:** Price, 24h volume, market cap, 7d/30d change %, ATH/ATL, supply
- **Rate limits:** 2s delay between coin calls (10-30 calls/min free tier)

### 2c. Real Estate (Weekly, Sundays)
- **Source:** FRED API (macro) + Zillow Research CSV (ZHVI by metro)
- **Data:** ZHVI by metro, 30yr mortgage rate, housing starts, Case-Shiller index
- **Challenge:** Mostly US data. UK needs HM Land Registry or ONS (Phase 2)

### 2d. News/Sentiment (Phase 2)
- NewsAPI or RSS feeds from financial news
- Deferred — start without, add based on value

---

## Component 3: Analysis Engine (Weekly)

The LLM-powered brain. Runs Sunday evening:

1. Reads last 1-4 weeks of market data from SQLite
2. Computes quantitative signals: RSI, price vs MAs, volume spikes, ATH drawdown, WoW/MoM % change
3. Feeds everything into Hermes LLM with a structured prompt
4. Outputs structured JSON recommendations → written to `recommendations` table
5. Dashboard picks up new recommendations automatically on next page load

**Prompt rules:** Conservative, evidence-based, cite data, flag uncertainty, no hype, strict JSON output.

---

## Component 4: Digest Compiler & Delivery (Weekly)

Converts analysis JSON into Markdown digest and delivers via:
1. **Telegram** — primary channel
2. **Obsidian** — archived to [[Investments/Digests/]] for history
3. **Email** — optional future channel

---

## Component 5: Web Dashboard (FastAPI + Jinja2 + Chart.js)

### Why FastAPI, not React/Next.js
- Zero frontend build step — no npm, no node_modules
- Server-rendered HTML with Jinja2 templates
- Chart.js from CDN for price charts
- Single Python file, deployable as k3s pod
- Same PVC mount as Hermes — reads the live SQLite DB

### Pages
| Route | Content |
|-------|---------|
| `/` | Overview cards, latest recommendations, digest preview |
| `/stocks` | Watchlist with sparklines, RSI, % change |
| `/crypto` | 7d/30d trends, ATH drawdowns |
| `/real-estate` | Metro ZHVI trends, mortgage rates |
| `/asset/<symbol>` | Full detail: Chart.js price history, technicals, recommendation history |
| `/recommendations` | History, filterable by asset class/confidence |
| `/digest/<id>` | Past digest viewer |

### Deployment
- k3s pod mounting Hermes PVC (read-only)
- Internal access: `investments.homelab.internal`
- Optional external: Cloudflare Access (same pattern as hackathon site)
- Security: read-only DB connection, no write endpoints

---

## Component 6: Configuration

Single YAML at `/opt/data/investment_config.yaml`:

```yaml
stocks:
  watchlist: [AAPL, MSFT, GOOGL, NVDA, TSLA, AMZN, META, BRK-B]
  indices: [^GSPC, ^IXIC, ^DJI]

crypto:
  watchlist: [bitcoin, ethereum, solana, cardano, polkadot]

real_estate:
  metros: ["New York, NY", "Austin, TX", "Miami, FL"]

analysis:
  horizon_days: 30
  min_confidence: low
  risk_tolerance: moderate

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
- [ ] SQLite schema + Python DB module
- [ ] `investment_config.yaml`
- [ ] Stock data collector CronJob (daily)
- [ ] Verify data collection

### Phase 2: Crypto + Real Estate
- [ ] Crypto data collector CronJob (daily)
- [ ] Real estate data collector CronJob (weekly)
- [ ] Verify all pipelines

### Phase 3: Dashboard
- [ ] FastAPI app + Jinja2 templates
- [ ] Overview, stock detail, crypto, real estate pages
- [ ] Chart.js price history charts
- [ ] Dockerfile + k3s manifests, deploy

### Phase 4: Analysis Engine
- [ ] Quantitative signal computation
- [ ] LLM analysis prompt + structured output
- [ ] Analysis CronJob → recommendations table
- [ ] Dashboard shows recommendations automatically

### Phase 5: Digest & Delivery
- [ ] Markdown digest compiler
- [ ] Telegram delivery CronJob
- [ ] Obsidian archival
- [ ] End-to-end test

### Phase 6: Polish
- [ ] Dark theme + mobile responsive dashboard
- [ ] News/sentiment integration
- [ ] Price drop alerts (CronJob threshold checker → Telegram)
- [ ] Portfolio tracking (holdings, allocation drift)
- [ ] Recommendation performance tracking

---

## Open Questions

1. **UK or US real estate?** FRED/Zillow is US-only. UK needs HM Land Registry or ONS data. Which markets — or both?
2. **Portfolio tracking?** Should the system know your actual holdings, or just recommend from watchlists?
3. **CronJob chaining:** Fixed schedule (simpler) or automatic trigger after collectors complete?
4. **Backtesting:** Track past recommendation accuracy? Adds complexity but provides accountability.

---

## Risks & Mitigations

| Risk | Mitigation |
|------|------------|
| yfinance API breaks | Community-maintained; fallback to Alpha Vantage free tier |
| CoinGecko rate limits | 2s delay between calls; fallback to CoinMarketCap |
| Real estate data US-only | Start US; add UK ONS later |
| LLM hallucinates financial advice | Prompt rules: cite data, flag uncertainty, mandatory disclaimer |
| CronJob failures | Telegram failure alerts |
| SQLite corruption | Periodic backups to R2 |
| SQLite lock contention | WAL mode; collectors are cron-scheduled, dashboard reads are sub-ms |
| Dashboard can't reach DB | Same PVC mount — no network |
| Dashboard exposed to internet | Internal-only by default; Cloudflare Access if external needed |

---

## ⚠️ Disclaimer

This is an automated system powered by AI. **Not financial advice.** Do your own research before making investment decisions. Past performance ≠ future results.

---

*Linked from: [[Current AI Trends - May 2026]] — a system built on Homelab for personal use.*
