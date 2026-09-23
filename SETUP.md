# daz-ai public runner template

This directory contains the exact files to place in a **public** repository named `daz-ai-runner-`.

The public repository is only an execution shell. It must never contain manuscript text, generated manuscript output, API keys, or PAT values.

## Why this works

GitHub standard hosted runners are free for public repositories. The public workflow uses a fine-grained PAT to check out the private `airuiwsk/daz-ai` repository, runs the existing naturalizer there, then commits only `books/<book_id>/naturalized/` back to the private repository.

## Public repository contents

Copy `naturalize.yml` to:

```
.github/workflows/naturalize.yml
```

A short public README is sufficient. Do not copy manuscripts or the private repository into the public repository.

## Required Actions secrets

Create exactly these two repository secrets in the public repository:

### GEMINI_API_KEY

The Gemini API key from Google AI Studio.

### DAZ_AI_PAT

Use a **fine-grained personal access token**, limited to only:

- Repository access: `airuiwsk/daz-ai`
- Repository permission: **Contents — Read and write**
- Metadata read access is implicit/required by GitHub

Do not grant administration, issues, actions, packages, or organization-wide access.

## Security rules

- Workflow trigger is `workflow_dispatch` only.
- Do not add `pull_request_target`.
- Do not print secrets.
- Do not upload manuscript artifacts.
- Do not commit manuscript contents to this public repository.
- Keep `permissions: contents: read` for the public repository's own `GITHUB_TOKEN`.
- The private-repo PAT exists only as an Actions secret.
- Delete/rotate `DAZ_AI_PAT` immediately if the public workflow is modified by an untrusted party.

## First run

Actions -> Gemini Japanese Naturalization Runner -> Run workflow

Use:

```
book_id: B20260913-101
force: false
```

On success, the private repository receives:

```
books/B20260913-101/naturalized/
  chapter1.md
  chapter1.md.naturalization.json
  ...
  manifest.json
```

The public repository receives no manuscript files.

## Resume behavior

If Gemini quota/error stops halfway, successful files are committed to the private repository. Re-run with `force=false`; files whose source hash is unchanged and already accepted are skipped.
