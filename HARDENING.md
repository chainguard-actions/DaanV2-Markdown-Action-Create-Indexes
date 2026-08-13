<!-- markdownlint-disable -->

# Hardening Report: DaanV2--Markdown-Action-Create-Indexes/v5.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **DaanV2--Markdown-Action-Create-Indexes/v5.0.0** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key and no job-level `permissions:` key on any job. Without explicit permissions, the GITHUB_TOKEN is granted its default (potentially broad) permissions. Add a top-level `permissions:` block with the minimal required scopes (e.g., `contents: read`).

Locations:

- `.github/workflows/pull-request.yml:1`

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key and no job-level `permissions:` key on any job. Without explicit permissions, the GITHUB_TOKEN is granted its default (potentially broad) permissions. Add a top-level `permissions:` block with the minimal required scopes (e.g., `contents: read`).

Locations:

- `.github/workflows/test-script.yml:1`

### unpinned-uses (severity: high)

Two `uses:` references are pinned to mutable tag refs instead of immutable 40-character commit SHAs, making the workflow vulnerable to supply-chain attacks if the tag is moved or the repository is compromised:
- `uses: actions/checkout@v4` (line 14) — should be pinned to a full SHA
- `uses: actions/setup-node@v4` (line 18) — should be pinned to a full SHA

Locations:

- `.github/workflows/pull-request.yml:14`
- `.github/workflows/pull-request.yml:18`

### unpinned-uses (severity: high)

Two `uses:` references are pinned to mutable branch refs (`@main`) instead of immutable 40-character commit SHAs, making the workflow vulnerable to supply-chain attacks if the branch is force-pushed or the repository is compromised:
- `uses: actions/checkout@main` (line 22) — should be pinned to a full SHA
- `uses: DaanV2/Markdown-Action-Create-Indexes@main` (line 25) — should be pinned to a full SHA

Locations:

- `.github/workflows/test-script.yml:22`
- `.github/workflows/test-script.yml:25`

## Iteration Notes

### Iteration 1

**Fixes applied:** missing-permissions, unpinned-uses

**Notes:**

Fixed both workflow files: (1) Added `permissions: contents: read` top-level block to pull-request.yml and test-script.yml to address missing-permissions findings. (2) Pinned all unpinned action references to full commit SHAs: actions/checkout@v4 → SHA 11d5960a..., actions/setup-node@v4 → SHA 49933ea5..., actions/checkout@main → SHA f548e57e..., DaanV2/Markdown-Action-Create-Indexes@main → SHA 50e78000.... Original tag/branch names preserved as inline comments.

