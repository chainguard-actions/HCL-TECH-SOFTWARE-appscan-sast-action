<!-- markdownlint-disable -->

# Hardening Report: HCL-TECH-SOFTWARE--appscan-sast-action/v1.1.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **HCL-TECH-SOFTWARE--appscan-sast-action/v1.1.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All three workflow files reference external actions using mutable tags instead of full 40-character commit SHAs. This exposes the workflow to supply-chain attacks where a tag can be silently moved to point to malicious code.

- codesweep.yml: `actions/checkout@v2`, `HCL-TECH-SOFTWARE/appscan-codesweep-action@v2`
- codesweep_publish.yml: `HCL-TECH-SOFTWARE/appscan-codesweep-action@v2`
- sast_github_action.yml: `actions/checkout@v3`, `HCL-TECH-SOFTWARE/appscan-sast-action@v1.0.1`

All should be pinned to a full SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v3`.

Locations:

- `.github/workflows/codesweep.yml:10`
- `.github/workflows/codesweep.yml:13`
- `.github/workflows/codesweep_publish.yml:8`
- `.github/workflows/sast_github_action.yml:14`
- `.github/workflows/sast_github_action.yml:16`

### missing-permissions (severity: medium)

codesweep.yml and codesweep_publish.yml have no top-level `permissions:` key and no job-level `permissions:` key on any of their jobs. Without explicit permissions, the GITHUB_TOKEN is granted its default (often broad) permissions, violating the principle of least privilege. A `permissions:` block with minimal required scopes should be added to each workflow.

Locations:

- `.github/workflows/codesweep.yml:1`
- `.github/workflows/codesweep_publish.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 5 unpinned action references across 3 workflow files by pinning each to its full 40-character commit SHA (with original tag preserved as a comment). Added top-level permissions blocks (contents: read, pull-requests: write) to codesweep.yml and codesweep_publish.yml, which previously had no permissions restrictions. The sast_github_action.yml already had a permissions block so only its action references needed pinning.

