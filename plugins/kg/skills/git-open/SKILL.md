---
name: git-open
description: >
  Open an existing Git repository and get its current state.
  WHEN: User wants to check repo status, verify repository, get branch info, see if working directory is clean.
  WHEN NOT: Need to find repo first (use git_discover), creating new repo (use git_init).
version: 0.1.0
---

# git_open - Open Repository

## Core Concept

`mcp__plugin_kg_kodegen__git_open` opens an existing Git repository and returns its current state including branch name and clean/dirty status.

## Key Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| path | string | Yes | - | Path to repository |

## Usage Examples

### Open repository
```json
{
  "path": "/path/to/repo"
}
```

### Open current directory
```json
{
  "path": "."
}
```

## Output Information

- `branch`: Current branch name (or "detached HEAD")
- `is_clean`: true if no uncommitted changes
- `path`: Confirmed repository path

## Working Directory Status

| Status | Meaning |
|--------|---------|
| clean | All changes committed |
| dirty | Uncommitted changes exist |

## Common Use Cases

1. **Verify context**: Confirm working with correct repo
2. **Pre-operation check**: Ensure clean state before pull/merge
3. **Branch detection**: Get current branch for conditional logic
4. **Workflow start**: First step in git operations

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Get repo info | git_open | Quick state check |
| Find repo root | git_discover | Search from nested path |
| Detailed status | git_status | Full change information |
| View history | git_log | Commit list |

## Remember

- Read-only operation, safe to call repeatedly
- Errors if path doesn't exist or isn't a git repo
- Use is_clean to check before operations needing clean state
- "detached HEAD" means not on a branch
- First step in most git workflows
