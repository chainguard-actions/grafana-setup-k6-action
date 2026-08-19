<!-- markdownlint-disable -->

# Hardening Report: grafana--setup-k6-action/v1.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **grafana--setup-k6-action/v1.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple action references are pinned to mutable version tags instead of immutable full 40-character SHA digests, making the workflow vulnerable to supply-chain attacks if the tag is moved.

In .github/workflows/check-dist.yaml:
- uses: actions/checkout@v4
- uses: actions/setup-node@v4
- uses: actions/upload-artifact@v4

In .github/workflows/test.yaml:
- uses: actions/checkout@v4 (appears 3 times)

All of these should be pinned to a full commit SHA, e.g. actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4

Locations:

- `.github/workflows/check-dist.yaml:28`
- `.github/workflows/check-dist.yaml:33`
- `.github/workflows/check-dist.yaml:57`
- `.github/workflows/test.yaml:7`
- `.github/workflows/test.yaml:16`
- `.github/workflows/test.yaml:28`

### missing-permissions (severity: medium)

The workflow file .github/workflows/test.yaml has no top-level `permissions:` key and none of its three jobs (protocol, browser, use-latest-release) define job-level `permissions:` blocks. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad (write access to contents, packages, etc.). A minimal permissions block such as `permissions: contents: read` should be added at the top level or per job.

Locations:

- `.github/workflows/test.yaml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all mutable action references to full 40-character SHA digests with version tag comments: actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262, actions/setup-node@v4 → @49933ea5288caeca8642d1e84afbd3f7d6820020, actions/upload-artifact@v4 → @ea165f8d65b6e75b540449e92b4886f43607fa02. Added top-level `permissions: contents: read` to test.yaml (check-dist.yaml already had a permissions block).

