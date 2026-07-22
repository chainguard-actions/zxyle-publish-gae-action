<!-- markdownlint-disable -->

# Hardening Report: zxyle--publish-gae-action/v1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **zxyle--publish-gae-action/v1.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow file uses `zxyle/publish-gae-action@master`, which is pinned to a mutable branch ref (`master`) rather than an immutable 40-character commit SHA. This means the action code can change at any time without notice, creating a supply-chain attack risk.

Locations:

- `.github/workflows/main.yml:9`

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key, and the single job (`build`) also has no `permissions:` key. Without explicit permissions, the workflow inherits the default repository permissions (which may include `write` access to contents, packages, etc.), violating the principle of least privilege.

Locations:

- `.github/workflows/main.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both findings in .github/workflows/main.yml: (1) Pinned `zxyle/publish-gae-action@master` to its full commit SHA `09db96d2cf1cdfb1d100d6a5bb1b0bc10fc7f7ca` with a `# master` comment for readability. (2) Added `permissions: {}` at the top level to enforce least-privilege — the workflow only runs `gcloud version` and needs no GitHub token permissions.

