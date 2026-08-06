<!-- markdownlint-disable -->

# Hardening Report: HCL-TECH-SOFTWARE--appscan-sast-action/v1.0.7

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **HCL-TECH-SOFTWARE--appscan-sast-action/v1.0.7** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference actions using mutable tags or version strings instead of pinned 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the referenced tag is moved or the repository is compromised. Failing references: codesweep.yml — `actions/checkout@v2`, `HCL-TECH-SOFTWARE/appscan-codesweep-action@v2`; codesweep_publish.yml — `HCL-TECH-SOFTWARE/appscan-codesweep-action@v2`; sast_github_action.yml — `actions/checkout@v3`, `HCL-TECH-SOFTWARE/appscan-sast-action@v1.0.1`.

Locations:

- `.github/workflows/codesweep.yml:10`
- `.github/workflows/codesweep.yml:13`
- `.github/workflows/codesweep_publish.yml:8`
- `.github/workflows/sast_github_action.yml:8`
- `.github/workflows/sast_github_action.yml:10`

### missing-permissions (severity: medium)

None of the three workflow files define a top-level `permissions:` block, and no job within them defines job-level permissions either. Without explicit permissions, workflows run with the default (potentially broad) token permissions, violating the principle of least privilege. Affected files: codesweep.yml, codesweep_publish.yml, sast_github_action.yml.

Locations:

- `.github/workflows/codesweep.yml:1`
- `.github/workflows/codesweep_publish.yml:1`
- `.github/workflows/sast_github_action.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 5 unpinned action references by replacing mutable tags with full 40-character commit SHAs (original tags preserved as comments). Added top-level permissions blocks to all three workflow files: codesweep.yml and codesweep_publish.yml get 'contents: read' + 'pull-requests: write' (needed to post PR comments/annotations); sast_github_action.yml gets 'contents: read' only (workflow_dispatch with no PR interaction).

