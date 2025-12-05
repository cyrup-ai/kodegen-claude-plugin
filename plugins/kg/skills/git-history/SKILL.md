---
name: git-history
description: >
  Investigate how a file changed over time with actual diffs.
  WHEN: User asks how file evolved, when code was added/removed, wants file history with diffs, "blame" style investigation.
  WHEN NOT: Just listing commits (use git_log), comparing two points (use git_diff).
version: 0.1.0
---

# git_history - File Evolution with Diffs

## Core Concept

`mcp__plugin_kg_kodegen__git_history` shows how a specific file changed over time, including actual diff content for each commit. Search for when specific code was added, removed, or modified.

## Key Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| path | string | Yes | - | Path to repository |
| file | string | Yes | - | File path to investigate |
| limit | number | No | 20 | Maximum commits to return |
| search | string | No | null | Regex to filter diffs |
| since | string | No | HEAD | Start revision |
| until | string | No | null | End revision (enables range mode) |

## Usage Examples

### See recent changes to file
```json
{
  "path": ".",
  "file": "src/auth.rs"
}
```

### Find when code was modified
```json
{
  "path": ".",
  "file": "src/auth.rs",
  "search": "validate_token"
}
```

### Find when code was added
```json
{
  "path": ".",
  "file": "src/auth.rs",
  "search": "^\\+.*new_function"
}
```

### Find when code was removed
```json
{
  "path": ".",
  "file": "src/auth.rs",
  "search": "^-.*deprecated"
}
```

### Compare two versions
```json
{
  "path": ".",
  "file": "src/auth.rs",
  "since": "v1.0",
  "until": "v2.0"
}
```

### Limit results
```json
{
  "path": ".",
  "file": "src/auth.rs",
  "limit": 5
}
```

## Output Modes

1. **Commits mode** (default): List of commits with diffs
2. **Range mode** (with `until`): Cumulative diff between two points

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| File history + diffs | git_history | Shows actual changes |
| Commit list only | git_log | Faster, no diff content |
| Compare two points | git_diff | Any files, not file-specific |
| Search code | fs_search | Current state, not history |

## Remember

- Shows commit summaries WITH actual +/- diffs inline
- Use `search` regex to filter for specific changes
- `^\\+` matches added lines, `^-` matches removed lines
- Read-only and idempotent operation
- Limit results for large histories
