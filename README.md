# daz-ai-runner-

Public GitHub Actions runner for the private Kindle Factory repository `airuiwsk/daz-ai`.

## Purpose

This repository contains execution configuration and non-sensitive validation status only.

It does **not** store:

- manuscripts
- Gemini manuscript outputs
- API keys
- private-repository credentials

Naturalization checks out the private repository with a fine-grained PAT, runs Gemini 3.8 Flash Japanese Naturalization, validates the result, and commits accepted output back to the private repository.

## Automatic schedule

Naturalization runs every day:

| JST | Book |
|---|---|
| 16:10 | B20260913-101 |
| 16:30 | B20260913-102 |
| 16:50 | B20260913-103 |

Scheduled runs use resume behavior: files already accepted with the same source hash are skipped.

Manual `Run workflow` remains available for recovery, debugging, or another Book ID.

## Validation

`Validate Private Kindle Factory` checks the private repository without calling Gemini.

It runs:
- Python compile check
- Genre Router tests
- selection / manifest route test
- Naturalizer source-ordering tests
- genre-aware QA smoke test

The latest result is stored in `validation/latest.md`. This file contains test output and a private-repo commit SHA only; it does not contain manuscript text.

## Required repository secrets

- `GEMINI_API_KEY`
- `DAZ_AI_PAT`

See [SETUP.md](SETUP.md).

## Security

- Naturalization trigger: fixed schedules + `workflow_dispatch`
- Validation trigger: restricted main-branch paths + `workflow_dispatch`
- Naturalization public-repo token: `contents: read`
- Validation public-repo token: `contents: write` only so it can update `validation/latest.md`
- Private PAT scope: only `airuiwsk/daz-ai`, Contents read/write
- No manuscript artifacts are uploaded to this public repository
- No secret values are printed or committed
- Do not use `pull_request_target`
