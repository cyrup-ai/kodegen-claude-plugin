---
name: git-remote-remove
description: >
  Remove a configured remote repository from Git configuration.
  WHEN: User says "remove remote", "delete remote", "disconnect remote", "drop remote".
  WHEN NOT: Listing remotes (use git_remote_list), updating remote URL (use git_remote_add with force).
version: 0.1.0
---

# Git Remote Remove

## Core Concept

`mcp__plugin_kg_kodegen__git_remote_remove` deletes a remote from repository configuration. This removes the remote name and URL but does not delete any remote-tracking branches.

## Key Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| path | string | Yes | - | Path to the Git repository |
| name | string | Yes | - | Name of remote to remove |

## Usage Examples

### Remove upstream after contributing
```json
{
  "path": ".",
  "name": "upstream"
}
```

### Remove old origin
```json
{
  "path": "/project",
  "name": "origin"
}
```

### Remove backup remote
```json
{
  "path": ".",
  "name": "backup"
}
```

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Remove remote | git_remote_remove | Deletes remote config |
| List remotes first | git_remote_list | Verify remote exists |
| Update remote URL | git_remote_add (force) | Keeps remote, changes URL |
| Add replacement | git_remote_add | After removing old |

## Common Scenarios

1. **After fork contribution**: Remove upstream remote when done
2. **Migration**: Remove old origin before adding new one
3. **Cleanup**: Remove test or temporary remotes
4. **Repository reorganization**: Clean up outdated remotes

## Remember

- Verify remote exists with git_remote_list first
- Does not delete remote-tracking branches (use git branch -dr)
- Cannot be undone - must re-add with git_remote_add
- Fails if remote name doesn't exist
- Local branches created from remote remain intact
