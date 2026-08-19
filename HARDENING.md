<!-- markdownlint-disable -->

# Hardening Report: grafana--setup-k6-action/v1.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **grafana--setup-k6-action/v1.1.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable version tags (e.g. @v4) instead of full 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the tag is moved to a malicious commit. Affected references: actions/checkout@v4, actions/setup-node@v4, actions/upload-artifact@v4.

Locations:

- `.github/workflows/check-dist.yaml:28`
- `.github/workflows/check-dist.yaml:34`
- `.github/workflows/check-dist.yaml:55`
- `.github/workflows/linux-runner-test.yaml:7`
- `.github/workflows/linux-runner-test.yaml:16`
- `.github/workflows/linux-runner-test.yaml:25`
- `.github/workflows/macos-runner-test.yaml:7`
- `.github/workflows/macos-runner-test.yaml:16`
- `.github/workflows/macos-runner-test.yaml:25`
- `.github/workflows/windows-runner-test.yaml:7`
- `.github/workflows/windows-runner-test.yaml:16`
- `.github/workflows/windows-runner-test.yaml:25`
- `.github/workflows/move-major-release-tag.yaml:12`

### missing-permissions (severity: medium)

These workflow files have no top-level `permissions:` key and no job-level `permissions:` key on any of their jobs. Without explicit permissions, workflows inherit the repository's default token permissions (which may be write-all), violating the principle of least privilege.

Locations:

- `.github/workflows/linux-runner-test.yaml:1`
- `.github/workflows/macos-runner-test.yaml:1`
- `.github/workflows/windows-runner-test.yaml:1`
- `.github/workflows/move-major-release-tag.yaml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all unpinned action references by replacing mutable @v4 tags with full commit SHAs: actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262, actions/setup-node@v4 → @49933ea5288caeca8642d1e84afbd3f7d6820020, actions/upload-artifact@v4 → @ea165f8d65b6e75b540449e92b4886f43607fa02. Added top-level `permissions:` blocks to the 4 workflows missing them: linux-runner-test.yaml, macos-runner-test.yaml, and windows-runner-test.yaml got `contents: read`; move-major-release-tag.yaml got `contents: write` (required to push tags). check-dist.yaml already had a permissions block and only needed the action pins fixed.

