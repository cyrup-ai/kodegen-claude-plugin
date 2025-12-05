---
name: git-fetch
description: >
  Download objects and refs from a remote repository without merging.
  WHEN: User wants to see what's on remote, update tracking branches, prepare for merge, sync remote refs.
  WHEN NOT: Want to also merge changes (use git_pull), pushing changes (use git_push).
version: 0.1.0
---

# git_fetch - Download Remote Updates

## Core Concept

`mcp__plugin_kg_kodegen__git_fetch` downloads objects and refs from a remote repository WITHOUT modifying your working directory or current branch. It updates remote-tracking branches (e.g., origin/main).

## Key Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| path | string | Yes | - | Path to repository |
| remote | string | No | "origin" | Remote name to fetch from |
| refspecs | string[] | No | [] | Specific refs to fetch |
| prune | boolean | No | false | Remove stale tracking branches |

## Usage Examples

### Standard fetch
```json
{
  "path": ".",
  "remote": "origin"
}
```

### Fetch and prune deleted branches
```json
{
  "path": ".",
  "remote": "origin",
  "prune": true
}
```

### Fetch from upstream (fork workflow)
```json
{
  "path": ".",
  "remote": "upstream"
}
```

### Fetch specific refspecs
```json
{
  "path": ".",
  "remote": "origin",
  "refspecs": ["refs/heads/main:refs/remotes/origin/main"]
}
```

## Fetch vs Pull

| Operation | What it does | Modifies branch? |
|-----------|--------------|------------------|
| fetch | Downloads only | No |
| pull | Downloads + merges | Yes |

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Download without merge | git_fetch | Safe, non-destructive |
| Download and merge | git_pull | Combines fetch + merge |
| Push to remote | git_push | Upload local commits |
| View remote branches | git_branch_list | After fetch shows updated refs |

## Remember

- Fetch is NON-DESTRUCTIVE: only adds data
- Safe to run multiple times (idempotent)
- Use `prune: true` to clean up deleted remote branches
- Fetch before pull to inspect changes first
- Updates origin/* branches but not local branches
