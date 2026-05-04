# Personal study analysis notes

> This directory is personal documentation for `Py-CI-Park/prism-insight-study`. It is not upstream project documentation. It exists to help one maintainer study the code, track upstream changes, and record how the program works before applying it locally.

- Upstream repository: `dragon1086/prism-insight`
- Baseline branch: `upstream/main`
- Initial baseline commit: `f396ce54802d`
- Initial baseline tag: `v2.12.0-4-gf396ce5`
- Baseline commit subject: `Merge pull request #271 from dragon1086/fix/exchange-code-dynamic-lookup`
- Created: 2026-05-04

## Purpose

These notes have three goals.

1. Keep a clean personal fork that can follow the latest upstream code.
2. Document the PRISM-INSIGHT process from code evidence, not memory.
3. Preserve a safe path for local experiments without mixing them into `main`.

## Document map

| Document | Purpose |
| --- | --- |
| [`WORKFLOW.md`](WORKFLOW.md) | Fork, branch, sync, commit, and update workflow. |
| [`PROCESS-MAP.md`](PROCESS-MAP.md) | First-pass map of the program process and important files. |
| [`APPLYING-LOCALLY.md`](APPLYING-LOCALLY.md) | Safe local application sequence and risk checklist. |
| [`FOCUS-QUEUE.md`](FOCUS-QUEUE.md) | Study backlog for future detailed notes. |
| [`templates/analysis-note.md`](templates/analysis-note.md) | Template for new analysis notes. |
| [`decisions/0001-repo-strategy.md`](decisions/0001-repo-strategy.md) | Decision record for using a separate official fork. |

## Recommended reading order

1. Read `README_ko.md`, `README.md`, `CLAUDE.md`, and `docs/SETUP_ko.md` for product-level context.
2. Read [`PROCESS-MAP.md`](PROCESS-MAP.md) to understand the major runtime flows.
3. Compare `stock_analysis_orchestrator.py` with `prism-us/us_stock_analysis_orchestrator.py`.
4. Study `trigger_batch.py` and `prism-us/us_trigger_batch.py` for candidate selection.
5. Study `cores/analysis.py` and `cores/agents/*.py` for LLM analysis sections.
6. Study `stock_tracking_agent.py`, `prism-us/us_stock_tracking_agent.py`, `tracking/`, and `trading/` for trading, tracking, journal, and safety behavior.
7. Reproduce one flow with `--no-telegram`, `--dry-run`, or `demo.py` before touching real credentials.

## Documentation rules

- Every note records the upstream commit it was based on.
- Separate code evidence from personal interpretation.
- Prefer safe commands first: `--no-telegram`, `--dry-run`, and demo commands.
- Never commit account credentials, tokens, generated databases, or local secret files.
