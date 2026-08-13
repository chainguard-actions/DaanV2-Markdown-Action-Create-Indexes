<!-- markdownlint-disable -->

# Hardening Report: DaanV2--Markdown-Action-Create-Indexes/v4.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **DaanV2--Markdown-Action-Create-Indexes/v4.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Both workflow files use mutable tag/branch refs instead of pinned 40-character commit SHAs, making the action vulnerable to supply-chain attacks if the referenced tags or branches are moved or compromised.

.github/workflows/pull-request.yml:
  - Line 13: `uses: actions/checkout@v4` (mutable tag)
  - Line 16: `uses: actions/setup-node@v4` (mutable tag)

.github/workflows/test script.yml:
  - Line 26: `uses: actions/checkout@main` (mutable branch)
  - Line 29: `uses: DaanV2/Markdown-Action-Create-Indexes@main` (mutable branch)

All should be pinned to full 40-character commit SHAs, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/pull-request.yml:13`
- `.github/workflows/pull-request.yml:16`
- `.github/workflows/test script.yml:26`
- `.github/workflows/test script.yml:29`

### missing-permissions (severity: medium)

Neither workflow file defines a top-level `permissions:` block, and neither job within them defines a job-level `permissions:` block. Without explicit permissions, workflows run with the default (potentially write) token permissions, violating the principle of least privilege.

- .github/workflows/pull-request.yml: no top-level or job-level permissions defined.
- .github/workflows/test script.yml: no top-level or job-level permissions defined.

Locations:

- `.github/workflows/pull-request.yml:1`
- `.github/workflows/test script.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files:

.github/workflows/pull-request.yml:
- Added `permissions: {}` top-level block
- Pinned `actions/checkout@v4` → `actions/checkout@11d5960a326750d5838078e36cf38b85af677262 # v4`
- Pinned `actions/setup-node@v4` → `actions/setup-node@49933ea5288caeca8642d1e84afbd3f7d6820020 # v4`

.github/workflows/test script.yml:
- Added `permissions: {}` top-level block
- Pinned `actions/checkout@main` → `actions/checkout@f548e57e544e1ff5a4c46bf1e1b8685f8e4a348a # main`
- Pinned `DaanV2/Markdown-Action-Create-Indexes@main` → `DaanV2/Markdown-Action-Create-Indexes@50e78000c3856beb86658e9d1a1683361a2f707c # main`

All SHAs were resolved using lookup_action_sha and are real commit SHAs, not invented values.

