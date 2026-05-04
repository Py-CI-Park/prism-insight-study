# PRISM-INSIGHT process map

- Baseline upstream commit: `f396ce54802d74aa23e8ed8f88a9158d9daa385f`
- Baseline tag: `v2.12.0-4-gf396ce5`
- Created: 2026-05-04
- Status: first-pass map; expand function-level details in follow-up notes.

## One-line summary

PRISM-INSIGHT is a Korean and US stock analysis and trading automation system. It gathers market data, selects candidate stocks, runs AI analysis agents, creates reports, sends optional notifications, and can connect the resulting decisions to tracking, journals, and KIS trading APIs.

## High-level flow

```text
configuration and secrets
  -> market data collection and trigger candidates
  -> macro context and per-stock AI analysis
  -> Markdown, HTML, and PDF reports
  -> Telegram, dashboard, or messaging delivery
  -> holding tracking and buy/sell decisions
  -> optional KIS orders or watchlist records
  -> journal, memory compression, and performance tracking
```

## File map by flow

| Flow | KR path | US path | Responsibility |
| --- | --- | --- | --- |
| Pipeline orchestration | `stock_analysis_orchestrator.py` | `prism-us/us_stock_analysis_orchestrator.py` | Runs trigger, macro, report, PDF, and Telegram stages. |
| Candidate triggers | `trigger_batch.py` | `prism-us/us_trigger_batch.py` | Selects candidates by volume, gap, value, sector, and contrarian rules. |
| Per-stock analysis | `cores/analysis.py` | `prism-us/cores/us_analysis.py` | Runs the analysis sections for one stock. |
| Agent prompts | `cores/agents/*.py` | `prism-us/cores/agents/*.py` | Defines market, company, news, technical, and trading agents. |
| Reports and Q&A | `report_generator.py` | Shared plus US-specific paths | Saves reports, handles cache/PDF, and supports evaluation or follow-up answers. |
| Telegram bot | `telegram_ai_bot.py` | Shared commands include US flows | Handles user commands, daily limits, conversation context, and responses. |
| Tracking decisions | `stock_tracking_agent.py` | `prism-us/us_stock_tracking_agent.py` | Reads reports, checks holdings/prices, decides buy/skip/watchlist/sell paths. |
| Trading API wrapper | `trading/domestic_stock_trading.py` | `prism-us/trading/us_stock_trading.py` | Wraps KIS API price, balance, order, and multi-account behavior. |
| Journal and memory | `tracking/journal.py`, `tracking/user_memory.py`, `compress_trading_memory.py` | `prism-us/tracking/*` | Stores decisions, lessons, and compressed memory. |
| Messaging | `messaging/*`, `firebase_bridge.py` | Shared | Provides Redis, GCP Pub/Sub, and Firebase delivery paths. |
| Performance | `performance_tracker_batch.py`, `performance_analysis_report.py`, `weekly_insight_report.py` | `prism-us/us_performance_tracker_batch.py` | Tracks trigger and trading performance. |

## KR orchestrator stages

`StockAnalysisOrchestrator` in `stock_analysis_orchestrator.py` is the main KR pipeline object.

1. `run_macro_intelligence()` prepares macro and market-regime context.
2. `run_trigger_batch()` selects candidate stocks for the current time window.
3. `generate_reports()` creates per-stock analysis reports.
4. `convert_to_pdf()` converts reports to PDF.
5. `generate_telegram_messages()` creates Telegram-ready summaries.
6. `send_telegram_messages()` and `send_trigger_alert()` send optional notifications.
7. `run_full_pipeline()` ties these stages together.

## US orchestrator stages

`USStockAnalysisOrchestrator` mirrors the KR flow but uses US-specific data and trading modules. Start with these files:

- `prism-us/cores/us_data_client.py`
- `prism-us/cores/us_social_sentiment.py`
- `prism-us/cores/us_surge_detector.py`
- `prism-us/trading/us_stock_trading.py`
- `prism-us/us_stock_tracking_agent.py`

## Trigger batch study path

Candidate selection is centered on `trigger_batch.py` and `prism-us/us_trigger_batch.py`.

1. Load snapshots or market data.
2. Apply liquidity and minimum-value filters.
3. Generate trigger-specific candidate lists:
   - `trigger_morning_volume_surge()`
   - `trigger_morning_gap_up_momentum()`
   - `trigger_morning_value_to_cap_ratio()`
   - `trigger_afternoon_daily_rise_top()`
   - `trigger_afternoon_closing_strength()`
   - `trigger_afternoon_volume_surge_flat()`
   - `trigger_macro_sector_leader()`
   - `trigger_contrarian_value()`
4. Score agent fit:
   - `calculate_agent_fit_metrics()`
   - `score_candidates_by_agent_criteria()`
5. Allocate final slots:
   - `_get_regime_slots()`
   - `_build_topdown_pool()`
   - `select_final_tickers()`
6. Execute the batch through `run_batch()`.

## AI agent structure

Important files:

- `cores/analysis.py`
- `cores/agents/company_info_agents.py`
- `cores/agents/market_index_agents.py`
- `cores/agents/news_strategy_agents.py`
- `cores/agents/stock_price_agents.py`
- `cores/agents/trading_agents.py`
- `cores/agents/trading_journal_agents.py`

Important rule: the repo guide warns not to parallelize LLM-heavy report sections casually. Prompt order, rate limits, and section dependencies matter.

## Trading and tracking flow

```text
trigger candidates
  -> analysis report
  -> tracking agent reads report, price, holdings, and account scope
  -> buy, skip, watchlist, or sell decision
  -> optional KIS order or watchlist record
  -> journal and memory update
```

Study these files first:

- `stock_tracking_agent.py`
- `prism-us/us_stock_tracking_agent.py`
- `tracking/journal.py`
- `trading/domestic_stock_trading.py`
- `prism-us/trading/us_stock_trading.py`

## Telegram and user-command flow

`telegram_ai_bot.py` owns interactive user workflows:

- report and evaluation commands;
- US report and US evaluation commands;
- conversation context;
- Firecrawl search and follow-up questions;
- daily limit accounting and refunds;
- calls into `report_generator.py`.

## Safe local commands

Use commands with no notification or dry-run behavior first.

```powershell
# KR morning pipeline without Telegram delivery
python stock_analysis_orchestrator.py --mode morning --no-telegram

# US morning pipeline without Telegram delivery
python prism-us/us_stock_analysis_orchestrator.py --mode morning --no-telegram

# Single-stock demos
python demo.py 005930
python demo.py AAPL --market us

# Weekly report dry run
python weekly_insight_report.py --dry-run
```

## Follow-up notes to write

1. Detailed trigger algorithm notes.
2. `StockAnalysisOrchestrator.run_full_pipeline()` call graph.
3. KR and US agent comparison table.
4. Buy/sell decision prompt and safety-guard notes.
5. DB schema, journal, and memory notes.
6. Telegram command matrix.
7. Local credential and account-safety checklist.
