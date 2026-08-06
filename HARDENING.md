<!-- markdownlint-disable -->

# Hardening Report: HCL-TECH-SOFTWARE--appscan-sast-action/v1.0.4

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **HCL-TECH-SOFTWARE--appscan-sast-action/v1.0.4** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All three workflow files reference GitHub Actions using mutable version tags instead of full 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the referenced tag is moved or the repository is compromised.

- codesweep.yml: `actions/checkout@v2`, `HCL-TECH-SOFTWARE/appscan-codesweep-action@v2`
- codesweep_publish.yml: `HCL-TECH-SOFTWARE/appscan-codesweep-action@v2`
- sast_github_action.yml: `actions/checkout@v3`, `HCL-TECH-SOFTWARE/appscan-sast-action@v1.0.1`

All should be pinned to a full SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v3`.

Locations:

- `.github/workflows/codesweep.yml:10`
- `.github/workflows/codesweep.yml:13`
- `.github/workflows/codesweep_publish.yml:9`
- `.github/workflows/sast_github_action.yml:9`
- `.github/workflows/sast_github_action.yml:11`

### missing-permissions (severity: medium)

None of the three workflow files declare a top-level `permissions:` block, and no job within them declares job-level permissions either. Without explicit permissions, GitHub Actions grants the default token permissions (which may include write access to repository contents and other resources depending on the organisation/repository settings). Each workflow should declare the minimal required permissions, e.g. `permissions: read-all` at minimum, or specific scopes such as `contents: read`.

Affected files:
- .github/workflows/codesweep.yml
- .github/workflows/codesweep_publish.yml
- .github/workflows/sast_github_action.yml

Locations:

- `.github/workflows/codesweep.yml:1`
- `.github/workflows/codesweep_publish.yml:1`
- `.github/workflows/sast_github_action.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 3 workflow files:
1. unpinned-uses: Pinned all 5 action references to full 40-char commit SHAs with original tags as comments: actions/checkout@v2→0717577d, appscan-codesweep-action@v2→ec3e8f3a (×2), actions/checkout@v3→a37ce912, appscan-sast-action@v1.0.1→d119873d.
2. missing-permissions: Added top-level permissions blocks to all 3 files — codesweep.yml and codesweep_publish.yml get 'contents: read' + 'pull-requests: write' (needed for PR annotations); sast_github_action.yml gets 'contents: read' only (workflow_dispatch scan only needs to read code).

