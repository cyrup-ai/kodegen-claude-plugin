---
name: git-remote-list
description: >
  List all configured remote repositories with their URLs.
  WHEN: User asks "what remotes exist", "show remotes", "list remotes", "which remotes", "remote URLs".
  WHEN NOT: Adding remote (use git_remote_add), removing remote (use git_remote_remove).
version: 0.1.0
---

# Git Remote List

## Core Concept

`mcp__plugin_kg_kodegen__git_remote_list` enumerates all configured remote repositories, showing their names and fetch/push URLs.

## Key Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| path | string | Yes | - | Path to the Git repository |

## Usage Examples

### List remotes in current repository
```json
{
  "path": "."
}
```

### List remotes in specific repository
```json
{
  "path": "/home/user/projects/my-app"
}
```

## Output Format

Returns JSON with:
- `success`: boolean
- `count`: number of remotes
- `remotes`: array with `name`, `fetch_url`, `push_url` for each

Human-readable shows consolidated URL if fetch and push are identical.

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| List all remotes | git_remote_list | Shows names and URLs |
| Add a remote | git_remote_add | Creates new remote |
| Remove a remote | git_remote_remove | Deletes remote |
| Verify remote before push | git_remote_list | Confirm correct URL |

## Remember

- Shows both fetch and push URLs (may differ in some setups)
- Empty list if no remotes configured
- Check remotes before git_remote_add to avoid conflicts
- Common remote names: origin, upstream, backup
- URLs may be HTTPS, SSH, or file:// protocol
