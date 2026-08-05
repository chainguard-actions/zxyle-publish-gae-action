<!-- markdownlint-disable -->

# Hardening Report: zxyle--publish-gae-action/v2.3.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **zxyle--publish-gae-action/v2.3.0** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Two `uses:` references in .github/workflows/main.yml are pinned to mutable tag or branch refs instead of immutable 40-character commit SHAs, making the workflow vulnerable to supply-chain attacks if those refs are moved:
- `actions/checkout@v1` (line 9) — mutable version tag
- `zxyle/publish-gae-action@master` (line 12) — mutable branch name

Locations:

- `.github/workflows/main.yml:9`
- `.github/workflows/main.yml:12`

### permissions (severity: medium)

missing-permissions: .github/workflows/main.yml has no top-level `permissions:` key and the single job `build` also has no `permissions:` key. Without explicit permissions, the workflow inherits the default repository token permissions, which may be overly broad.

Locations:

- `.github/workflows/main.yml:1`

### script-injection (severity: high)

Sub-rule (a): A `${{ ... }}` expression is interpolated directly inside a `run:` shell command string. On line 22, `${{ secrets.GCP_SA_EMAIL }}` is embedded directly in the `gcloud auth activate-service-account` command. GitHub Actions performs YAML template substitution before the shell ever sees the string, so any newlines or shell metacharacters in the value are passed raw to the shell. The value should be passed via an `env:` variable and then double-quoted in the shell script. Offending line: `gcloud auth activate-service-account ${{ secrets.GCP_SA_EMAIL }} --key-file=client-secret.json`

Locations:

- `.github/workflows/main.yml:22`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, permissions, script-injection

**Notes:**

Fixed all three findings in .github/workflows/main.yml: (1) Pinned actions/checkout@v1 to SHA 50fbc622fc4ef5163becd7fab6573eac35f8462e and zxyle/publish-gae-action@master to SHA 09db96d2cf1cdfb1d100d6a5bb1b0bc10fc7f7ca, preserving original refs in comments. (2) Added top-level `permissions: {}` to deny all default token permissions. (3) Moved `${{ secrets.GCP_SA_EMAIL }}` from the run: shell command into an env: block as GCP_SA_EMAIL, and referenced it as double-quoted "$GCP_SA_EMAIL" in the shell script to prevent script injection.

