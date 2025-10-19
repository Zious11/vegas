# DK‑First NFL Betting Signal Generator — Implementation Plan

This plan turns the method in `nfl-betting-method-draftkings-first-las-vegas.md` into a practical, DK‑first signal system that emits manual bet signals (no auto‑betting).

## 1) Objectives and Scope
- **Primary goal**: Generate actionable signals with price, EV, fractional Kelly sizing, and expiry, so you can place bets manually.
- **Secondary goals**: Track Closing Line Value (CLV), produce weekly reports, and refine thresholds.
- **Non‑goals**: Auto‑betting, evading geofence/limits, or guaranteeing profits.

## 2) System Overview
- **Inputs**: DraftKings odds (primary benchmark), Vegas locals (Circa, Westgate, Caesars/WH, BetMGM, STN, South Point, Boyd) via API or manual CSV.
- **Core Engine**: Normalizes odds, computes implied probability, EV, fractional Kelly, identifies Wong‑eligible teasers, and filters by thresholds.
- **Outputs**: Signals as CSV/JSONL, Slack alerts (optional), and persistence in SQLite for audit/CLV.
- **Schedules**: Openers, mid‑week, T−90 (inactives), and pre‑close snapshot for CLV.

```mermaid
flowchart LR
  A[Odds Providers (DK + Locals)] --> B[Normalizer]
  B --> C[EV/Kelly/Teaser Rules]
  C --> D[Signal Filter + Thresholds]
  D --> E[Outputs CSV/JSON/Slack]
  D --> F[SQLite Persistence]
  F --> G[CLV Tracker (Close Snapshot)]
```

## 3) Data Sources
- **DraftKings (primary)**: Use a reputable odds aggregator (e.g., TheOddsAPI, SportsDataIO). Configure via env var key. No scraping.
- **Locals (NV)**: Prefer aggregator coverage. MVP fallback: manual CSV exports placed in `data/locals_csv/` with columns:
  - `event_id,date,book,market,selection,price,points,total`
- **Event IDs**: Normalize home/away and kickoff timestamps to create stable `event_id` keys (e.g., `NFL2025-WK8-NE@BUF`).

- **Note on NV locals availability**: Most public odds APIs do not include Nevada-only books (Circa, Westgate, STN, South Point, Boyd). For visibility, use paid screens (e.g., SpankOdds, Don Best) and transcribe snapshots into the CSV fallback at scheduled times.

## 4) Configuration (YAML)
```yaml
books:
  primary: draftkings
  locals: [circa, westgate, caesars_wh, mgm, stn, south_point, boyd]
odds_providers:
  dk: {type: aggregator, name: the_odds_api, api_key: ${ODDS_API_KEY}}
  locals: {type: aggregator, name: the_odds_api}
  locals_csv_dir: data/locals_csv/
thresholds:
  min_ev: 0.0125        # 1.25% EV baseline for sides/totals
  props_min_ev: 0.02    # higher bar for props
  dk_vs_local_min_cents: 5
  teaser_price_max: -120  # global max; per-book overrides below
teasers:
  price_by_book:
    draftkings: -120
    circa: -120
    westgate: -130
  require_cross_3_and_7: true
  points: 6
  max_total: 49
risk:
  unit_size_pct: 0.0075   # 0.75% of bankroll per standard wager
  kelly_fraction: 0.25     # 25% Kelly on strongest edges
closing_line:
  benchmark_order: [dk, circa, composite]
  composite_books: [betmgm, caesars_wh, fanduel, pointsbet]
alerts:
  slack_webhook: ${SLACK_WEBHOOK_URL}
storage:
  sqlite_path: data/signals.db
data:
  respect_provider_tos: true
  rate_limit_qps: 2
scheduling:
  jobs: [openers, midweek, t_minus_90, close_snapshot]
```

## 5) Core Calculations
- **Implied probability (American)**
  - Minus: `p_imp = |odds| / (|odds| + 100)`
  - Plus: `p_imp = 100 / (|odds| + 100)`
- **EV per $1 (decimal d, fair p)**: `EV = p*(d − 1) − (1 − p)`
- **Kelly fraction (decimal)**: `b = d − 1; k = (b*p − (1 − p))/b`; clamp to `[0, k_max]`; apply fractional Kelly.

## 6) Signal Rules
- **Price Discrepancy (DK vs Local)**
  - If a local price improves DK by ≥ `dk_vs_local_min_cents` or improves a key number (e.g., −2.5 vs −3), signal.
- **Model Edge (Fair vs Price)**
  - If `EV ≥ min_ev` (sides/totals) or `EV ≥ props_min_ev` (props), signal. DK is preferred execution; in NV, execute locally if price ≥ DK.
- **Wong Teasers**
  - Two‑team 6‑pt; each leg crosses 3 and 7; require a valid teaser price. Only emit when the per-book teaser price exists and is ≤ `thresholds.teaser_price_max` (or equal/better than `teasers.price_by_book[book]` when specified). Prefer lower totals (e.g., < 49).
- **Props/Derivatives**
  - Injury/role/weather sensitive. Use higher EV thresholds and smaller unit caps.
- **Expiry**
  - Signals expire at kickoff or if price shifts beyond edge (re‑evaluate on next snapshot).

## 7) Schedules
- **Openers** (Sun night/Mon): Capture early mispricings.
- **Mid‑week** (Wed–Fri): React to practice/injury reports.
- **Gameday T−90**: Inactives—highest edge window.
- **Pre‑close**: T−5 to T0 snapshot for CLV.

## 8) Outputs
- Files: `signals/YYYY‑MM‑DD_signals.jsonl` and `.csv` with stable IDs.
- SQLite tables:
  - `odds_snapshots(event_id, book, market, selection, price, ts)`
  - `signals(id, event_id, market, selection, dk_price, local_best_book, local_best_price, fair_prob, ev, kelly_fraction, suggested_units, valid_until, ts)`
  - `executions(signal_id, stake_units, book, price, ts)`
  - `closing_lines(event_id, market, selection, dk_close, local_close_book, local_close_price, ts)`
- Slack alert payload (example):
```json
{
  "title": "Signal: BUF -2.5 (-105)",
  "event": "NE@BUF",
  "benchmark": "DK -110",
  "best_local": {"book": "Circa", "price": -105},
  "ev": 0.015,
  "kelly_fraction": 0.06,
  "suggested_units": 0.5,
  "expires": "2025-10-19T20:30:00Z"
}
```

## 9) CLV Tracking
- For each signal, record the close using `closing_line.benchmark_order` (e.g., DK → Circa → composite). Store `close_price` and `close_source`. If no official close is available, use the last pre‑kick snapshot with source noted.
- Weekly report: distribution of CLV deltas and realized ROI to validate process.

## 10) Risk & Thresholds
- Start conservative: `min_ev` 1–1.5% for sides/totals, 2–3% for props. Max unit per play 0.5–1.0% bankroll.
- Apply fractional Kelly only on strongest signals; cap suggested units.

## 11) Error Handling & Logging
- Structured logs to `logs/` with request IDs and provider response codes.
- Graceful backoff on API errors; skip a cycle rather than emit stale signals.

## 12) Security & Compliance
- Store API keys in env vars; never commit secrets.
- Respect geofencing and book terms. Respect provider ToS/licensing and adhere to rate limits; cache responses where appropriate. No scraping that violates ToS.

## 13) Project Layout
```
signals_engine/
  src/
    datasources/
      draftkings.py
      locals.py
    normalize/markets.py
    engine/ev.py
    engine/kelly.py
    engine/teasers.py
    engine/signals.py
    engine/clv_tracker.py
    outputs/csv_json.py
    outputs/slack.py
    outputs/sqlite.py
    cli.py
  config/config.yaml
  data/locals_csv/         # manual fallback for NV locals
  signals/
  logs/
  tests/
```

## 14) Technology Choices
- **Language**: Python 3.11+
- **Libs**: `requests`, `pydantic`, `PyYAML`, `APScheduler`, `click`, `pandas` (optional), `slack_sdk`, built‑in `sqlite3`.

## 15) Milestones (2–3 weeks)
- **M1 (MVP, week 1)**
  - Aggregator client for DK + one local; CSV fallback loader.
  - Normalizer + EV/Kelly + signal rules (sides/totals, teasers).
  - CLI: `fetch`, `signal`, `close-snapshot`.
  - Outputs: CSV/JSONL + SQLite persistence.
- **M2 (week 2)**
  - Slack alerts, props module, improved event matching, robust logging.
  - CLV weekly report; initial threshold tuning.
- **M3 (week 3)**
  - Add more locals/APIs, refine teaser logic, add tests and fixtures.
  - Dry‑run through one NFL week; calibrate EV/unit caps.

## 16) Acceptance Criteria
- Signals emitted with stable IDs, suggested units, and expiry.
- Ability to reproduce signals from SQLite and snapshots.
- Weekly CLV report shows that signals, on average, beat the close.

- Teaser signals only emitted when a valid per-book teaser price is present and acceptable by config.
- Each CLV record includes `close_price` and `close_source` (dk|circa|composite); proxy order honored.
- Locals CSV snapshot present for each scheduled run where API coverage is absent.

## 17) Validation & Revisions
- **Validated logistics**: DraftKings not live in Nevada (mobile); NV requires in‑person registration. Plan uses DK as benchmark, locals for execution in NV.
- **Data reality**: Public APIs cover DK and major nationals, not NV locals. Plan includes paid screens (SpankOdds/Don Best) and manual CSV fallback.
- **Timing confirmed**: NFL inactives at T−90 (NFL Operations). Scheduler targets openers/mid‑week/T−90/close.
- **Method underpinnings**: CLV predicts skill/profitability (Pinnacle/Unabated); fractional Kelly for risk control (Thorp/Unabated). Wong teasers only through 3 & 7 at fair pricing (≈ −120), avoid −130/−140.
- **Adjustments added**: Per‑book teaser pricing config, CLV proxy order with `close_source`, ToS/rate‑limit compliance, locals CSV requirement.

## 18) Operating Procedure (Manual Execution)
- Schedule jobs or run CLI targets around key windows.
- Review `signals/*.csv` sorted by EV/time; place bets manually.
- Log executions (stake, book, price) via CLI or CSV append.

## 19) Future Enhancements
- Add more books/providers; integrate weather/injury feeds.
- Portfolio constraints (exposure by team/market/day).
- Web dashboard for signals and CLV analytics.

---
- This plan complements the method in `nfl-betting-method-draftkings-first-las-vegas.md` and is designed for DK‑first pricing with Nevada execution constraints.
