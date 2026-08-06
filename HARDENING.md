<!-- markdownlint-disable -->

# Hardening Report: HCL-TECH-SOFTWARE--appscan-sast-action/v1.0.6

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **HCL-TECH-SOFTWARE--appscan-sast-action/v1.0.6** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references in the workflow files use mutable tags or version strings instead of pinned 40-character SHA commit hashes. This exposes the action to supply-chain attacks where a tag could be silently moved to point to malicious code. Failing references:
- `.github/workflows/codesweep.yml`: `actions/checkout@v2`, `HCL-TECH-SOFTWARE/appscan-codesweep-action@v2`
- `.github/workflows/codesweep_publish.yml`: `HCL-TECH-SOFTWARE/appscan-codesweep-action@v2`
- `.github/workflows/sast_github_action.yml`: `actions/checkout@v3`, `HCL-TECH-SOFTWARE/appscan-sast-action@v1.0.1`

Locations:

- `.github/workflows/codesweep.yml:9`
- `.github/workflows/codesweep.yml:12`
- `.github/workflows/codesweep_publish.yml:8`
- `.github/workflows/sast_github_action.yml:9`
- `.github/workflows/sast_github_action.yml:11`

### missing-permissions (severity: medium)

None of the three workflow files define a top-level `permissions:` block, and no job within them defines job-level `permissions:` either. Without explicit permissions, workflows run with the default (often broad) token permissions, violating the principle of least privilege. Affected files: codesweep.yml, codesweep_publish.yml, sast_github_action.yml.

Locations:

- `.github/workflows/codesweep.yml:1`
- `.github/workflows/codesweep_publish.yml:1`
- `.github/workflows/sast_github_action.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 5 unpinned action references across 3 workflow files by replacing mutable tags with full 40-character SHA commit hashes (preserving tags as comments). Added top-level permissions blocks to all 3 workflow files with minimal required permissions: codesweep.yml and codesweep_publish.yml get 'contents: read' and 'pull-requests: write' (needed for PR interaction), while sast_github_action.yml gets only 'contents: read' (only needs to checkout code for scanning).

