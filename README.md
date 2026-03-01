# MarketPulse Training Runner

Public GitHub Actions runner repository for MarketPulse model retraining.

## Purpose
- Execute training on a public-repo runner (free minutes model) while keeping the main trading repo private.
- Clone the private target repository at runtime with a PAT secret.
- Run `kaggle/train_all.py` and push artifacts back to `model-artifacts` in the private repo.

## Required Repository Secrets
Set these in this public repo (`Settings -> Secrets and variables -> Actions`):

- `TARGET_REPO_PAT`:
  - GitHub PAT with `repo` scope.
  - Must have access to `kwon2540/MarketPulse-Railway`.
- `DATABASE_URL`:
  - Public PostgreSQL connection URL usable from GitHub-hosted runners.

## Workflow
- File: `.github/workflows/train-models-from-private.yml`
- Triggers:
  - Manual (`workflow_dispatch`)
  - Scheduled every 4 hours
- Modes:
  - `lite`: xgboost,catboost
  - `full`: xgboost,catboost,transformer,informer,tabnet

## Important Notes
- Do not add `pull_request` trigger in this public repo for this workflow.
- Secrets are not exposed to forks by default, but keep branch protections enabled.
