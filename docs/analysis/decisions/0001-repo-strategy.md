# 0001. Personal study repository strategy

- Decision date: 2026-05-04
- Baseline upstream commit: `f396ce54802d74aa23e8ed8f88a9158d9daa385f`

## Decision

Use `Py-CI-Park/prism-insight-study`, an official fork of `dragon1086/prism-insight`, as the personal study repository.

## Context

The old `Py-CI-Park/prism-insight` repository was not an official GitHub fork. The old local `feature/study` branch was also heavily diverged from `upstream/main`. That history may remain useful as an archive, but it is not a clean baseline for studying the current upstream process.

## Chosen approach

- Do not delete or rename the old repository.
- Create a separate official fork named `prism-insight-study`.
- Clone it to `C:\System_Trading\prism-insight-study`.
- Keep `main` aligned with upstream.
- Store personal analysis notes under `docs/analysis/` on `study/analysis-docs`.
- Disable the upstream push URL.

## Rejected alternatives

- Reuse the old `feature/study` branch: it is too diverged from upstream for a clean restart.
- Delete or rename the old personal repository: unnecessary for solo study and risky for old references.
- Use an upstream-only clone: it does not provide a durable personal GitHub location for notes.

## Consequence

The user gets a clean upstream-tracking baseline while preserving old material separately. Personal learning notes can evolve without contaminating `main`.
