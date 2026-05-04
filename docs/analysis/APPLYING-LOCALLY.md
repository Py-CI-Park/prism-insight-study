# Applying PRISM-INSIGHT locally

- Baseline upstream commit: `f396ce54802d74aa23e8ed8f88a9158d9daa385f`
- Created: 2026-05-04

## Principle

This project can connect to real messaging, real account data, and trading APIs. Apply it locally in layers.

1. Read: understand documentation and source structure.
2. Reproduce: run demo, dry-run, or `--no-telegram` commands.
3. Record: save observations under `docs/analysis/`.
4. Experiment: use a separate `experiment/*` branch for code changes.
5. Real credentials or live notifications: only after a separate checklist passes.

## Stage 1: pre-environment review

Check these files before creating secrets:

- `.env.example`
- `mcp_agent.config.yaml.example`
- `mcp_agent.secrets.yaml.example`
- `README_ko.md`
- `docs/SETUP_ko.md`

Never commit local secret files, API tokens, account IDs, generated databases, PDFs, or log output.

## Stage 2: safest commands first

```powershell
python demo.py 005930
python demo.py AAPL --market us
python weekly_insight_report.py --dry-run
```

Then use orchestrators with notification disabled.

```powershell
python stock_analysis_orchestrator.py --mode morning --no-telegram
python prism-us/us_stock_analysis_orchestrator.py --mode morning --no-telegram
```

## Stage 3: record each study session

For every run or code-reading session, record:

```md
- Baseline commit:
- Command executed:
- Input data:
- Generated files:
- Main files/functions called:
- Business rules understood:
- Unknowns:
- Next verification:
```

Use `docs/analysis/templates/analysis-note.md` as the starting point.

## Stage 4: code experiments

Do not change code on `study/analysis-docs`. Use a separate branch.

```powershell
git switch main
git pull --ff-only origin main
git switch -c experiment/local-config-study
```

Minimum verification after a code edit:

```powershell
python -m compileall <changed-python-files>
pytest <focused-test-file>
```

## Stage 5: live-action checklist

Before enabling real Telegram delivery, real KIS access, or live trading paths:

- [ ] I have a documented reason for not using `--no-telegram` or `--dry-run`.
- [ ] Demo or dry-run results were recorded in `docs/analysis/`.
- [ ] I know whether the KIS account is virtual or real.
- [ ] Buy amount, max slots, sector concentration, and stop-loss rules are understood.
- [ ] Failure behavior for orders and database updates is understood.
- [ ] `git status --short` does not show `.env`, secret YAML files, token files, SQLite files, reports, or logs.
