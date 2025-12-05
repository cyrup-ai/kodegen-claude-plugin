---
name: git-diff
description: >
  Show differences between Git revisions, branches, or working directory.
  WHEN: User asks "what changed", "show diff", wants to compare versions, review changes, see modifications.
  WHEN NOT: Need commit history (use git_log), need file content (use fs_read_file).
version: 0.1.0
---

# git_diff - Compare Changes

## Core Concept

`mcp__plugin_kg_kodegen__git_diff` shows differences between Git revisions. Compare commits, branches, or see uncommitted changes in the working directory.

## Key Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| path | string | Yes | - | Path to repository |
| from | string | Yes | - | Source revision (commit, branch, HEAD) |
| to | string | No | working dir | Target revision |

## Usage Examples

### Compare two commits
```json
{
  "path": ".",
  "from": "abc1234",
  "to": "def5678"
}
```

### Compare branches (PR preview)
```json
{
  "path": ".",
  "from": "main",
  "to": "feature-branch"
}
```

### See uncommitted changes
```json
{
  "path": ".",
  "from": "HEAD"
}
```

### Compare tagged versions
```json
{
  "path": ".",
  "from": "v1.0.0",
  "to": "v1.1.0"
}
```

### View specific commit changes
```json
{
  "path": ".",
  "from": "abc1234~1",
  "to": "abc1234"
}
```

## Output Format

- Change icons: added, deleted, modified, renamed
- Per-file statistics: +additions, -deletions
- Summary: total files changed and line counts

## Common Workflows

1. **Code review**: Compare feature branch to main before merge
2. **Uncommitted check**: See what you've modified before commit
3. **Release notes**: Compare tagged versions
4. **Bisect helper**: Review individual commits

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| See changes | git_diff | Shows actual differences |
| Commit history | git_log | Lists commits without diffs |
| File history | git_history | Shows how file evolved |
| Current status | git_status | Shows staged/unstaged files |

## Remember

- Omit `to` for working directory comparison
- Direction matters: from/to order affects perspective
- Use branch names over short hashes for clarity
- Renamed files appear as single entry
- Read-only operation, safe to call repeatedly
