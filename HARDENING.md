<!-- markdownlint-disable -->

# Hardening Report: HCL-TECH-SOFTWARE--appscan-sast-action/v1.0.9

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **HCL-TECH-SOFTWARE--appscan-sast-action/v1.0.9** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All three workflow files reference actions using mutable tags instead of pinned full-length SHA commit hashes, making them vulnerable to supply-chain attacks if the referenced tag is moved or the repository is compromised.

- codesweep.yml: `actions/checkout@v2` and `HCL-TECH-SOFTWARE/appscan-codesweep-action@v2`
- codesweep_publish.yml: `HCL-TECH-SOFTWARE/appscan-codesweep-action@v2`
- sast_github_action.yml: `actions/checkout@v3` and `HCL-TECH-SOFTWARE/appscan-sast-action@v1.0.1`

All should be pinned to a full 40-character commit SHA (e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v3`).

Locations:

- `.github/workflows/codesweep.yml:9`
- `.github/workflows/codesweep.yml:11`
- `.github/workflows/codesweep_publish.yml:8`
- `.github/workflows/sast_github_action.yml:9`
- `.github/workflows/sast_github_action.yml:11`

### missing-permissions (severity: medium)

None of the three workflow files define a top-level `permissions:` block, and none of the individual jobs define job-level `permissions:` blocks. Without explicit permissions, workflows inherit the default repository permissions (which may be `write-all`), granting unnecessarily broad access to the GITHUB_TOKEN. Each workflow should declare minimal required permissions (e.g. `permissions: read-all` or specific scopes like `contents: read`).

Locations:

- `.github/workflows/codesweep.yml:1`
- `.github/workflows/codesweep_publish.yml:1`
- `.github/workflows/sast_github_action.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all three workflow files:

1. codesweep.yml: Pinned `actions/checkout@v2` → `@0717577d45739eb3c851188b29f50ed6c0b2194e # v2` and `HCL-TECH-SOFTWARE/appscan-codesweep-action@v2` → `@ec3e8f3a32868329aa3d4e5662d4a58f2cee4476 # v2`. Added `permissions: contents: read, pull-requests: read`.

2. codesweep_publish.yml: Pinned `HCL-TECH-SOFTWARE/appscan-codesweep-action@v2` → `@ec3e8f3a32868329aa3d4e5662d4a58f2cee4476 # v2`. Added `permissions: contents: read, pull-requests: read`.

3. sast_github_action.yml: Pinned `actions/checkout@v3` → `@a37ce9120846195fa4ece8f58b268e6043cb2f26 # v3` and `HCL-TECH-SOFTWARE/appscan-sast-action@v1.0.1` → `@d119873d6403b00a578d8542fc55ee36974fa912 # v1.0.1`. Added `permissions: contents: read`.

