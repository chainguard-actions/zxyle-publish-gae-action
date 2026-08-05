<!-- markdownlint-disable -->

# Hardening Report: zxyle--publish-gae-action/v2.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **zxyle--publish-gae-action/v2.2** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow file references two Actions using mutable tags/branches instead of full 40-character commit SHA pins. `actions/checkout@v1` (line 9) uses a version tag, and `zxyle/publish-gae-action@master` (line 12) uses a branch name. Either reference could be silently updated to point to malicious code without any change to the workflow file, enabling a supply-chain attack.

Locations:

- `.github/workflows/main.yml:9`
- `.github/workflows/main.yml:12`

### script-injection (severity: high)

Sub-rule (a): A GitHub Actions expression `${{ secrets.GCP_SA_EMAIL }}` is interpolated directly inside a `run:` shell command string on line 22: `gcloud auth activate-service-account ${{ secrets.GCP_SA_EMAIL }} --key-file=client-secret.json`. Although `secrets.*` is not attacker-controlled in the same way as `github.*` or `inputs.*`, any `${{ ... }}` expression inside a `run:` block undergoes YAML template substitution before the shell ever sees it, bypassing shell quoting. The value should be passed via an `env:` variable and referenced as a quoted shell variable (e.g. `"$GCP_SA_EMAIL"`) instead.

Locations:

- `.github/workflows/main.yml:22`

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key and the single job `build` also has no `permissions:` key. Without explicit permissions, the workflow inherits the repository's default token permissions (which may be `write-all` on older repositories), granting the `GITHUB_TOKEN` broader access than necessary. A minimal `permissions:` block (e.g. `contents: read`) should be added.

Locations:

- `.github/workflows/main.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection, missing-permissions

**Notes:**

Fixed all three findings in .github/workflows/main.yml: (1) Pinned actions/checkout@v1 to SHA 50fbc622fc4ef5163becd7fab6573eac35f8462e and zxyle/publish-gae-action@master to SHA 09db96d2cf1cdfb1d100d6a5bb1b0bc10fc7f7ca, preserving original refs as comments. (2) Moved ${{ secrets.GCP_SA_EMAIL }} from the run: shell string into an env: block and referenced it as "$GCP_SA_EMAIL" in the shell command. (3) Added top-level permissions: contents: read block to enforce least-privilege token access.

