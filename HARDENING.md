<!-- markdownlint-disable -->

# Hardening Report: DaanV2--Markdown-Action-Create-Indexes/v5.0.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **DaanV2--Markdown-Action-Create-Indexes/v5.0.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references are pinned to mutable tags or branch names instead of full 40-character commit SHAs, making the workflow vulnerable to supply-chain attacks if the referenced action is compromised or the tag is moved.

`.github/workflows/pull-request.yml`:
- Line 14: `uses: actions/checkout@v4`
- Line 17: `uses: actions/setup-node@v4`

`.github/workflows/test-script.yml`:
- Line 23: `uses: actions/checkout@main`
- Line 26: `uses: DaanV2/Markdown-Action-Create-Indexes@main`

All should be pinned to a full SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/pull-request.yml:14`
- `.github/workflows/pull-request.yml:17`
- `.github/workflows/test-script.yml:23`
- `.github/workflows/test-script.yml:26`

### missing-permissions (severity: medium)

Neither workflow file defines a top-level `permissions:` key, and neither of their jobs defines a job-level `permissions:` key. Without explicit permissions, GitHub Actions grants the default token permissions (which may include `write` access to repository contents), violating the principle of least privilege. Both workflows should declare minimal required permissions explicitly.

Locations:

- `.github/workflows/pull-request.yml:1`
- `.github/workflows/test-script.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files:

**pull-request.yml**:
- Pinned `actions/checkout@v4` → `actions/checkout@11d5960a326750d5838078e36cf38b85af677262 # v4`
- Pinned `actions/setup-node@v4` → `actions/setup-node@49933ea5288caeca8642d1e84afbd3f7d6820020 # v4`
- Added `permissions: contents: read` at the top level

**test-script.yml**:
- Pinned `actions/checkout@main` → `actions/checkout@f548e57e544e1ff5a4c46bf1e1b8685f8e4a348a # main`
- Pinned `DaanV2/Markdown-Action-Create-Indexes@main` → `DaanV2/Markdown-Action-Create-Indexes@50e78000c3856beb86658e9d1a1683361a2f707c # main`
- Added `permissions: contents: read` at the top level

Both workflows only need `contents: read` to check out the repository and run build/test steps.

