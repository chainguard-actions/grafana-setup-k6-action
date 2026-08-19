<!-- markdownlint-disable -->

# Hardening Report: grafana--setup-k6-action/v0.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **grafana--setup-k6-action/v0.0.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow file uses `actions/checkout@v4` (a mutable version tag) in both jobs instead of a pinned 40-character commit SHA. This exposes the workflow to supply-chain attacks if the tag is moved to a different commit. Failing references: `actions/checkout@v4` at lines 9 and 17.

Locations:

- `.github/workflows/test.yaml:9`
- `.github/workflows/test.yaml:17`

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key, and neither job (`protocol` nor `browser`) defines its own `permissions:` block. Without explicit permissions, the workflow runs with the default (potentially broad) token permissions. Add a top-level `permissions: {}` or minimal per-job permissions to follow the principle of least privilege.

Locations:

- `.github/workflows/test.yaml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed .github/workflows/test.yaml: (1) Pinned both occurrences of `actions/checkout@v4` to `actions/checkout@11d5960a326750d5838078e36cf38b85af677262 # v4` to prevent supply-chain attacks from mutable tags. (2) Added `permissions: {}` at the top level to enforce least privilege, since the workflow only checks out code and runs k6 scripts with no GitHub token operations required.

