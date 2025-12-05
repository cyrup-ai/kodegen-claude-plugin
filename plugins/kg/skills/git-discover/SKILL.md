---
name: git-discover
description: >
  Find the Git repository root from any path within it.
  WHEN: User needs to find repo root, verify git repo exists, locate .git directory from nested path.
  WHEN NOT: Already know repo location (use git_open), checking if path is in repo (use git_open and handle error).
version: 0.1.0
---

# git_discover - Find Repository Root

## Core Concept

`mcp__plugin_kg_kodegen__git_discover` locates a Git repository by searching upward from a given path. It traverses parent directories until finding a .git directory or reaching filesystem root.

## Key Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| path | string | Yes | - | Path to search from |

## Usage Examples

### Find repo from nested directory
```json
{
  "path": "/home/user/project/src/components/deep/nested"
}
```

### Find repo from relative path
```json
{
  "path": "./src/lib"
}
```

### Verify current directory is in repo
```json
{
  "path": "."
}
```

## Use Cases

1. **Determine repository context**: Find containing repo for any file path
2. **Locate .git directory**: Get exact path to repository root
3. **Validate git presence**: Verify a path is within a Git repository

## Search Behavior

- Starts at provided path
- Walks upward through parent directories
- Stops at first .git/ directory found
- Errors if no repository found before filesystem root

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Find repo root | git_discover | Searches upward |
| Open known repo | git_open | Direct access |
| Clone new repo | git_clone | Downloads from remote |
| Init new repo | git_init | Creates fresh repo |

## Remember

- Read-only operation, never modifies filesystem
- Works from any subdirectory depth
- Cache result if calling multiple tools (repo root doesn't change)
- Errors if no repository exists in path hierarchy
- Requires read access to directory hierarchy
