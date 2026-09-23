# daz-ai-runner-

Public GitHub Actions runner for the private Kindle Factory repository `airuiwsk/daz-ai`.

## Purpose

This repository contains **execution configuration only**.

It does **not** store:

- manuscripts
- Gemini outputs
- API keys
- private-repository credentials

The workflow checks out the private repository with a fine-grained PAT, runs Gemini 3.8 Flash Japanese Naturalization, validates the result, and commits accepted output back to the private repository.

## Run

After the two Actions secrets below are configured:

1. Open **Actions**
2. Select **Gemini Japanese Naturalization Runner**
3. Choose **Run workflow**
4. Enter `B20260913-101`
5. Leave `force=false`

## Required repository secrets

- `GEMINI_API_KEY`
- `DAZ_AI_PAT`

See [SETUP.md](SETUP.md) for the one-time setup.

## Security

- Trigger: `workflow_dispatch` only
- Public-repo token permission: read only
- Private PAT scope: only `airuiwsk/daz-ai`, Contents read/write
- No manuscript artifacts are uploaded to this public repository


## Automatic schedule

The workflow runs automatically every day at **16:10 JST (07:10 UTC)**.

Scheduled runs currently target:

```
B20260913-101
force=false
```

Accepted files are skipped using source hashes, so completed chapters are not sent to Gemini again. Manual `Run workflow` remains available for debugging or another book ID.
