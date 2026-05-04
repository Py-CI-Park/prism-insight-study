# Personal fork workflow

- Initial baseline commit: `f396ce54802d74aa23e8ed8f88a9158d9daa385f`
- Initial baseline tag: `v2.12.0-4-gf396ce5`
- Created: 2026-05-04

## Repository topology

```text
origin   = https://github.com/Py-CI-Park/prism-insight-study.git
upstream = https://github.com/dragon1086/prism-insight.git
```

Roles:

- `origin`: my personal fork; push here.
- `upstream`: original project; read-only tracking source.
- `main`: clean baseline that follows `upstream/main`.
- `study/analysis-docs`: long-lived branch for personal analysis notes.

The `upstream` push URL is intentionally set to `DISABLED` to prevent accidental pushes.

## Regular upstream sync

### 1. Fetch upstream

```powershell
cd C:\System_Trading\prism-insight-study
git fetch upstream --prune --tags
```

### 2. Keep `main` aligned with upstream

Do not write personal notes directly on `main`.

```powershell
git switch main
git merge --ff-only upstream/main
git push origin main
```

If `--ff-only` fails, stop and inspect the branch because personal commits may have entered `main`.

### 3. Rebase personal docs on the latest baseline

For a solo branch, rebase keeps history simple.

```powershell
git switch study/analysis-docs
git rebase main
git push --force-with-lease origin study/analysis-docs
```

If another person starts using the branch, use merge instead of rebase.

```powershell
git merge main
git push origin study/analysis-docs
```

## Writing a new analysis note

1. Record the baseline commit.

```powershell
git rev-parse --short=12 upstream/main
git describe --tags --always upstream/main
```

2. Copy the template.

```powershell
Copy-Item docs/analysis/templates/analysis-note.md docs/analysis/<topic>.md
```

3. Cite concrete files and symbols.

```md
Evidence:
- `stock_analysis_orchestrator.py`: `StockAnalysisOrchestrator.run_full_pipeline()`
- `trigger_batch.py`: `run_batch()`, `select_final_tickers()`
```

4. Commit in small units.

```powershell
git add docs/analysis
git commit -m "Document <topic> study notes"
git push origin study/analysis-docs
```

## Commit message style

Use the repo's Lore style for useful decision history.

```text
Capture personal study workflow for upstream tracking

This records the fork-based workflow and initial process map so future
study notes stay tied to an upstream commit instead of the old diverged
local branch.

Constraint: Work is for solo study and should not modify upstream
Rejected: Reuse old feature/study branch | too diverged from upstream/main
Confidence: high
Scope-risk: narrow
Tested: git remote -v; git status --short --branch
Not-tested: full application runtime
```

## Branch names

| Purpose | Example |
| --- | --- |
| Personal analysis docs | `study/analysis-docs` |
| Topic-specific study | `study/trigger-system` |
| Local code experiment | `experiment/local-dry-run` |
| Possible upstream contribution | `fix/<short-topic>` or `docs/<short-topic>` |

## Conflict policy

- If upstream code and personal docs conflict, update the docs to match current code.
- Do not patch upstream code on the docs branch.
- Use a separate `experiment/*` branch for code changes.
