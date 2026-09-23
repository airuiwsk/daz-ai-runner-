# AI-Arbitrage Observer setup

The workflow is already installed at:

`.github/workflows/arbitrage-observer.yml`

It stays idle until the repository secret below exists.

## 1. Create a fine-grained PAT

GitHub account settings:

`Settings -> Developer settings -> Personal access tokens -> Fine-grained tokens -> Generate new token`

Recommended settings:

- Resource owner: `airuiwsk`
- Repository access: **Only select repositories**
- Repository: **AI-Arbitrage**
- Repository permissions:
  - **Contents: Read and write**
  - Metadata remains read-only automatically
- Expiration: 7-30 days is sufficient for this experiment

No wallet, exchange, Render, Actions-admin, or other repository permissions are required.

## 2. Add the secret to this public runner

Open:

`daz-ai-runner- -> Settings -> Secrets and variables -> Actions -> New repository secret`

Name:

`AI_ARBITRAGE_PAT`

Value: the fine-grained PAT from step 1.

The secret value is not written to workflow logs or source files.

## 3. Start

Open:

`Actions -> AI Arbitrage Observer -> Run workflow -> Run workflow`

The first successful run creates `data/observer/state.json` on the private
`AI-Arbitrage@telemetry` branch and starts a seven-day observation window.

After that, each long run self-dispatches the next one. A two-hour cron acts only
as a recovery watchdog if the handoff chain breaks.

## Storage

All durable output stays in the private repository:

- `AI-Arbitrage@telemetry:data/telemetry/YYYY-MM-DD/HH.jsonl`
- `AI-Arbitrage@telemetry:data/opportunities/`
- `AI-Arbitrage@telemetry:data/observer/state.json`

The public runner stores execution configuration only.

## Safety

The observer remains read-only:

- no transaction submission
- no wallet/private key
- no automatic trading
- no paid RPC
- `execution_enabled=false`
