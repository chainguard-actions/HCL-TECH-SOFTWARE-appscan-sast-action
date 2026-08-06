<!-- markdownlint-disable -->

# Hardening Report: HCL-TECH-SOFTWARE--appscan-sast-action/v1.1.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **HCL-TECH-SOFTWARE--appscan-sast-action/v1.1.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All three workflow files reference GitHub Actions using mutable tags or version strings instead of pinned 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the referenced tag is moved or the repository is compromised.

- codesweep.yml: `actions/checkout@v2` (line 10), `HCL-TECH-SOFTWARE/appscan-codesweep-action@v2` (line 13)
- codesweep_publish.yml: `HCL-TECH-SOFTWARE/appscan-codesweep-action@v2` (line 9)
- sast_github_action.yml: `actions/checkout@v3` (line 8), `HCL-TECH-SOFTWARE/appscan-sast-action@v1.0.1` (line 10)

All should be replaced with full 40-character hex commit SHAs, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v3`.

Locations:

- `.github/workflows/codesweep.yml:10`
- `.github/workflows/codesweep.yml:13`
- `.github/workflows/codesweep_publish.yml:9`
- `.github/workflows/sast_github_action.yml:8`
- `.github/workflows/sast_github_action.yml:10`

### missing-permissions (severity: medium)

None of the three workflow files define a top-level `permissions:` block, and none of the individual jobs define a job-level `permissions:` block. Without explicit permissions, workflows inherit the default repository permissions (which may be `write-all` for private repositories or broad read access for public ones), violating the principle of least privilege. Each workflow should declare the minimal permissions required, e.g. `permissions: read-all` or specific scopes such as `contents: read`.

Locations:

- `.github/workflows/codesweep.yml:1`
- `.github/workflows/codesweep_publish.yml:1`
- `.github/workflows/sast_github_action.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 3 workflow files: (1) Pinned all 5 action references to full 40-char commit SHAs with original tags preserved as comments — actions/checkout@v2→0717577d, appscan-codesweep-action@v2→ec3e8f3a (×2), actions/checkout@v3→a37ce912, appscan-sast-action@v1.0.1→d119873d. (2) Added top-level permissions blocks to all 3 files — codesweep.yml and codesweep_publish.yml get 'contents: read' + 'pull-requests: write' (needed for posting scan results on PRs); sast_github_action.yml gets 'contents: read' only (workflow_dispatch scan only needs to read code).

