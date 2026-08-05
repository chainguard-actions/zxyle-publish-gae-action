<!-- markdownlint-disable -->

# Hardening Report: zxyle--publish-gae-action/v2.3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **zxyle--publish-gae-action/v2.3** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow file references actions using mutable tags and branch names instead of full 40-character SHA commit hashes. `actions/checkout@v1` uses a version tag and `zxyle/publish-gae-action@master` uses a branch name. Both are vulnerable to supply-chain attacks where the referenced tag or branch could be updated to point to malicious code.

Locations:

- `.github/workflows/main.yml:9`
- `.github/workflows/main.yml:11`

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key and the single job `build` also has no `permissions:` key. Without explicit permissions, the workflow runs with the default (potentially broad) token permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/main.yml:1`

### script-injection (severity: high)

Sub-rule (a): A `${{ }}` expression is directly interpolated inside a `run:` shell command string. The step 'Print Google Cloud SDK version' uses `${{ secrets.GCP_SA_EMAIL }}` directly in the shell command: `gcloud auth activate-service-account ${{ secrets.GCP_SA_EMAIL }} --key-file=client-secret.json`. Any `${{ ... }}` expression inside a run: block is a script-injection risk because the value is substituted into the shell command string before the shell parses it. The value should be passed via an `env:` variable and referenced as a quoted shell variable instead.

Locations:

- `.github/workflows/main.yml:20`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions, script-injection

**Notes:**

Fixed all three findings in .github/workflows/main.yml: (1) Pinned actions/checkout@v1 to SHA 50fbc622fc4ef5163becd7fab6573eac35f8462e and zxyle/publish-gae-action@master to SHA 09db96d2cf1cdfb1d100d6a5bb1b0bc10fc7f7ca, preserving original refs as comments. (2) Added top-level `permissions: {}` to enforce least privilege. (3) Moved `${{ secrets.GCP_SA_EMAIL }}` from the run: shell string into the step's env: block as GCP_SA_EMAIL, and referenced it as the quoted shell variable "$GCP_SA_EMAIL" to prevent script injection.

