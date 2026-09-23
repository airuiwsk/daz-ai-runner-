# daz-ai public runner setup

This public repository is an execution shell for the private Kindle Factory repository `airuiwsk/daz-ai`.

It must never contain manuscript text, generated manuscript output, API keys, or PAT values.

## Required files

```text
.github/workflows/naturalize.yml
.github/workflows/validate-private.yml
README.md
SETUP.md
validation/latest.md      # generated validation result only
```

## Required Actions secrets

### GEMINI_API_KEY
Gemini API key used only by the Naturalization workflow.

### DAZ_AI_PAT
Fine-grained personal access token limited to:

- Repository access: `airuiwsk/daz-ai`
- Contents: Read and write
- Metadata: required implicit read access

Do not grant administration, issues, packages, organization-wide, or unrelated repository access.

## Scheduled Naturalization

The production workflow runs:

- 16:10 JST → `B20260913-101`
- 16:30 JST → `B20260913-102`
- 16:50 JST → `B20260913-103`

Each run uses resume behavior. Accepted files with an unchanged source hash are skipped, so completed books become effectively no-op runs.

A manual `workflow_dispatch` remains available. Use `force=false` unless intentionally re-naturalizing unchanged source.

## Validation workflow

`Validate Private Kindle Factory` checks the current private `main` branch using the existing `DAZ_AI_PAT`.

It performs compile checks, targeted unit tests, and a genre-aware QA smoke test. The result written to `validation/latest.md` contains only test logs and the tested private commit SHA.

The validation workflow does **not** call Gemini and does **not** publish private manuscript content.

## Security rules

- Never add manuscript text or Gemini output to this public repository.
- Never print or commit secrets.
- Never add `pull_request_target`.
- Naturalization workflow keeps its own `GITHUB_TOKEN` at `contents: read`.
- Validation uses `contents: write` only to commit `validation/latest.md`.
- The private PAT remains an Actions secret and is limited to `airuiwsk/daz-ai`.
- Rotate `DAZ_AI_PAT` immediately if workflow integrity is compromised.

## Manual recovery

Actions → Gemini Japanese Naturalization Runner → Run workflow

Example:

```text
book_id: B20260913-101
force: false
```

If Gemini quota/error stops midway, accepted checkpoint files are committed to the private repository. A later `force=false` run skips those accepted files and continues the remaining sources.
