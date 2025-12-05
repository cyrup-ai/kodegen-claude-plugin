---
name: git-log
description: >
  View commit history with author, date, and message information.
  WHEN: User wants commit log, history of changes, list commits, see who changed what.
  WHEN NOT: Need diffs for each commit (use git_history), comparing two points (use git_diff).
version: 0.1.0
---

# git_log - Commit History

## Core Concept

`mcp__plugin_kg_kodegen__git_log` displays commit history from a Git repository. Filter by file path, limit results, and paginate through history.

## Key Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| path | string | Yes | - | Path to repository |
| max_count | number | No | unlimited | Maximum commits to return |
| skip | number | No | 0 | Commits to skip (pagination) |
| path_filter | string | No | null | Filter by file/directory |

## Usage Examples

### View recent commits
```json
{
  "path": "."
}
```

### View last 10 commits
```json
{
  "path": ".",
  "max_count": 10
}
```

### Skip and paginate
```json
{
  "path": ".",
  "skip": 20,
  "max_count": 10
}
```

### Filter by file
```json
{
  "path": ".",
  "path_filter": "src/main.rs"
}
```

### Filter by directory
```json
{
  "path": ".",
  "path_filter": "docs/",
  "max_count": 5
}
```

## Output Format

Each commit includes:
- `id`: Full commit SHA
- `author.name`: Committer name
- `author.email`: Committer email
- `author.time`: Timestamp (RFC 3339)
- `summary`: Commit message first line
- `time`: Commit creation timestamp

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| List commits | git_log | Fast, no diff content |
| Commits + diffs | git_history | Shows actual changes |
| Compare versions | git_diff | Two-point comparison |
| Check status | git_status | Current state only |

## Remember

- Commits ordered newest to oldest
- Use max_count to limit results on large repos
- path_filter uses Git pathspec syntax (globs work)
- Pagination: combine skip + max_count
- Read-only and idempotent operation
