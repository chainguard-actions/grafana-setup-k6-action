<!-- markdownlint-disable -->

# Hardening Report: grafana--setup-k6-action/v1.2.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **grafana--setup-k6-action/v1.2.1** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (b) violation: The run block in move-major-release-tag.yaml uses env vars sourced from github.* context values but expands them unquoted in shell commands. Specifically:
- `git tag -fa ${MAJOR} -m "Update major version tag"` — ${MAJOR} is unquoted (derived from GITHUB_REF via `VERSION=${GITHUB_REF#refs/tags/}; MAJOR=${VERSION%%.*}`)
- `git push origin ${MAJOR} --force` — ${MAJOR} is unquoted
- `https://x-access-token:${GITHUB_TOKEN}@github.com/${GITHUB_REPOSITORY}.git` — ${GITHUB_REPOSITORY} is unquoted in a URL
An attacker who can create a release with a crafted tag name (e.g. containing shell metacharacters) could achieve command injection. All these variables should be double-quoted: `"${MAJOR}"` and `"${GITHUB_REPOSITORY}"`.

Locations:

- `.github/workflows/move-major-release-tag.yaml:19`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed unquoted shell variable expansions in hardened/action/.github/workflows/move-major-release-tag.yaml. Added double-quotes around all variable expansions: VERSION, MAJOR, GITHUB_REPOSITORY (in the git remote URL), and GITHUB_ACTOR. Specifically: `git tag -fa "${MAJOR}"`, `git push origin "${MAJOR}"`, and the full git remote URL is now quoted as `"https://x-access-token:${GITHUB_TOKEN}@github.com/${GITHUB_REPOSITORY}.git"`. This prevents shell word-splitting and potential command injection via crafted tag names or repository names.

