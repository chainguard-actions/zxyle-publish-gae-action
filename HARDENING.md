<!-- markdownlint-disable -->

# Hardening Report: zxyle--publish-gae-action/2.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **zxyle--publish-gae-action/2.1** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow uses `zxyle/publish-gae-action@master`, which is a mutable branch reference rather than a pinned 40-character commit SHA. This means the action could be silently updated (or compromised) without the workflow noticing, enabling supply-chain attacks.

Locations:

- `.github/workflows/main.yml:10`

### script-injection (severity: high)

Rule (a) violation: A `run:` block directly interpolates a `${{ ... }}` expression into a shell command string. The step 'Print Google Cloud SDK version' contains: `gcloud auth activate-service-account ${{ secrets.GCP_SA_EMAIL }} --key-file=client-secret.json`. GitHub Actions performs YAML template substitution before the shell processes the string, so any special characters in the substituted value are interpreted by the shell. The value should be passed via an `env:` variable and referenced as a quoted shell variable (e.g., `"$GCP_SA_EMAIL"`) instead.

Locations:

- `.github/workflows/main.yml:19`

### missing-permissions (severity: medium)

The workflow file `.github/workflows/main.yml` has no top-level `permissions:` key and no job-level `permissions:` key on any job. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad. A minimal `permissions:` block (e.g., `permissions: {}` or only the specific scopes needed) should be added.

Locations:

- `.github/workflows/main.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection, missing-permissions

**Notes:**

Fixed all three findings in .github/workflows/main.yml: (1) Pinned `zxyle/publish-gae-action@master` to the full commit SHA `09db96d2cf1cdfb1d100d6a5bb1b0bc10fc7f7ca` with a `# master` comment for readability. (2) Moved `${{ secrets.GCP_SA_EMAIL }}` from the `run:` shell string into an `env:` block as `GCP_SA_EMAIL`, then referenced it as `"$GCP_SA_EMAIL"` in the shell command to prevent script injection. (3) Added `permissions: {}` at the top level of the workflow to enforce least-privilege token permissions.

