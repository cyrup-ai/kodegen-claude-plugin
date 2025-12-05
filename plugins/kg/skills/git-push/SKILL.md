---
name: git-push
description: >
  Push local commits to a remote repository.
  WHEN: User wants to upload changes, push to origin, share commits, "git push".
  WHEN NOT: Just committing locally (use git_commit), downloading changes (use git_pull).
version: 0.1.0
---

# git_push - Push to Remote

## Core Concept

`mcp__plugin_kg_kodegen__git_push` uploads local commits and tags to a remote repository. Requires authentication (SSH keys or credentials).

## Key Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| path | string | Yes | - | Path to repository |
| remote | string | No | "origin" | Remote name |
| refspecs | string[] | No | [] | Refs to push (empty = current branch) |
| force | boolean | No | false | Force push (DANGEROUS) |
| tags | boolean | No | false | Push all tags |
| timeout_secs | number | No | 300 | Network timeout |

## Usage Examples

### Push current branch
```json
{
  "path": ".",
  "remote": "origin",
  "refspecs": []
}
```

### Push specific branches
```json
{
  "path": ".",
  "remote": "origin",
  "refspecs": ["main", "develop"]
}
```

### Push with tags
```json
{
  "path": ".",
  "remote": "origin",
  "refspecs": ["main"],
  "tags": true
}
```

### Force push (DANGEROUS)
```json
{
  "path": ".",
  "remote": "origin",
  "refspecs": ["feature-branch"],
  "force": true
}
```

## Authentication

- **SSH**: Keys in ~/.ssh/ with proper permissions
- **HTTPS**: Credential helper or personal access tokens
- Test with: `git ls-remote origin`

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Upload commits | git_push | Share with remote |
| Create commit | git_commit | Local only |
| Download | git_pull/git_fetch | Get from remote |

## WARNING - Force Push

- **NEVER** force push to shared/protected branches
- Force push overwrites remote history
- Can corrupt teammates' work
- Use only on personal feature branches after rebase

## Remember

- Empty refspecs pushes current branch
- Authentication required for remote access
- Non-fast-forward rejected unless force=true
- Use tags=true for releases
- Safe to push same commits multiple times (idempotent)
