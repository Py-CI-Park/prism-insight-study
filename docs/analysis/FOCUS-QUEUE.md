# Study backlog

- Baseline upstream commit: `f396ce54802d74aa23e8ed8f88a9158d9daa385f`
- Created: 2026-05-04

## Priority 1: whole-flow understanding

- [ ] Write a detailed call graph for `stock_analysis_orchestrator.py` and `run_full_pipeline()`.
- [ ] Compare `prism-us/us_stock_analysis_orchestrator.py` with the KR orchestrator.
- [ ] Map README commands to actual Python entrypoints.

## Priority 2: candidate selection

- [ ] Document input and output columns for each `trigger_batch.py` trigger.
- [ ] Explain `select_final_tickers()` and market-regime slot allocation.
- [ ] Explain US sector mapping and sector-leader behavior.

## Priority 3: AI agents and prompts

- [ ] Document the section order in `cores/analysis.py`.
- [ ] Summarize buy/sell rules in `cores/agents/trading_agents.py`.
- [ ] Document macro context from the macro intelligence agent.

## Priority 4: tracking and trading safety

- [ ] Document buy, skip, watchlist, and sell flow in `stock_tracking_agent.py`.
- [ ] Document KIS order parameters in `trading/domestic_stock_trading.py`.
- [ ] Document `tracking/journal.py` and memory compression.

## Priority 5: operations and channels

- [ ] Build a Telegram command matrix from `telegram_ai_bot.py`.
- [ ] Separate Redis, GCP Pub/Sub, and Firebase responsibilities.
- [ ] Review dashboard and frontend examples.
