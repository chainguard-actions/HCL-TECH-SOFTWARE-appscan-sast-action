<!-- markdownlint-disable -->

# Hardening Report: HCL-TECH-SOFTWARE--appscan-sast-action/v1.0.8

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **HCL-TECH-SOFTWARE--appscan-sast-action/v1.0.8** was hardened automatically. 6 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### missing-permissions (severity: medium)

Workflow file has no top-level `permissions:` key and no job-level `permissions:` key on any job. This means the workflow runs with the default (potentially broad) GITHUB_TOKEN permissions. A top-level or per-job `permissions:` block with minimal specific scopes should be added.

Locations:

- `.github/workflows/codesweep.yml:1`

### missing-permissions (severity: medium)

Workflow file has no top-level `permissions:` key and no job-level `permissions:` key on any job. This means the workflow runs with the default (potentially broad) GITHUB_TOKEN permissions. A top-level or per-job `permissions:` block with minimal specific scopes should be added.

Locations:

- `.github/workflows/codesweep_publish.yml:1`

### missing-permissions (severity: medium)

Workflow file has no top-level `permissions:` key and no job-level `permissions:` key on any job. This means the workflow runs with the default (potentially broad) GITHUB_TOKEN permissions. A top-level or per-job `permissions:` block with minimal specific scopes should be added.

Locations:

- `.github/workflows/sast_github_action.yml:1`

### unpinned-uses (severity: high)

Two `uses:` references are pinned to mutable tags rather than immutable 40-character SHA digests, making the workflow vulnerable to supply-chain attacks if the referenced tag is moved or overwritten: `actions/checkout@v2` and `HCL-TECH-SOFTWARE/appscan-codesweep-action@v2`.

Locations:

- `.github/workflows/codesweep.yml:9`
- `.github/workflows/codesweep.yml:13`

### unpinned-uses (severity: high)

One `uses:` reference is pinned to a mutable tag rather than an immutable 40-character SHA digest: `HCL-TECH-SOFTWARE/appscan-codesweep-action@v2`.

Locations:

- `.github/workflows/codesweep_publish.yml:8`

### unpinned-uses (severity: high)

Two `uses:` references are pinned to mutable tags rather than immutable 40-character SHA digests: `actions/checkout@v3` and `HCL-TECH-SOFTWARE/appscan-sast-action@v1.0.1`.

Locations:

- `.github/workflows/sast_github_action.yml:8`
- `.github/workflows/sast_github_action.yml:10`

## Iteration Notes

### Iteration 1

**Fixes applied:** missing-permissions, unpinned-uses

**Notes:**

Fixed all three workflow files: (1) Added `permissions: {}` top-level block to codesweep.yml, codesweep_publish.yml, and sast_github_action.yml to prevent broad default GITHUB_TOKEN permissions. (2) Pinned all unpinned action references to immutable 40-character SHA digests: actions/checkout@v2 → 0717577d45739eb3c851188b29f50ed6c0b2194e, actions/checkout@v3 → a37ce9120846195fa4ece8f58b268e6043cb2f26, HCL-TECH-SOFTWARE/appscan-codesweep-action@v2 → ec3e8f3a32868329aa3d4e5662d4a58f2cee4476, HCL-TECH-SOFTWARE/appscan-sast-action@v1.0.1 → d119873d6403b00a578d8542fc55ee36974fa912. Original tags preserved as inline comments for readability.

