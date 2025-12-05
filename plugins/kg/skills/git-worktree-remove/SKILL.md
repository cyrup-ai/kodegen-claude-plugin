---
name: git-worktree-remove
description: >
  Remove a worktree and its administrative files properly.
  WHEN: User says "remove worktree", "delete worktree", "clean up worktree", "done with worktree".
  WHEN NOT: Just unlocking (use git_worktree_unlock), cleaning stale entries (use git_worktree_prune).
version: 0.1.0
---

# Git Worktree Remove

## Core Concept

`mcp__plugin_kg_kodegen__git_worktree_remove` properly removes a worktree directory and its associated Git administrative files. This is the correct way to clean up worktrees.

## Key Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| path | string | Yes | - | Path to the main Git repository |
| worktree_path | string | Yes | - | Path to the worktree to remove |
| force | boolean | No | false | Force removal of locked worktrees |

## Usage Examples

### Remove completed feature worktree
```json
{
  "path": ".",
  "worktree_path": "../feature-complete"
}
```

### Force remove locked worktree
```json
{
  "path": "/main/repo",
  "worktree_path": "/stuck/worktree",
  "force": true
}
```

### Remove temporary worktree
```json
{
  "path": ".",
  "worktree_path": "../temp-experiment",
  "force": false
}
```

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Remove worktree properly | git_worktree_remove | Cleans directory and admin files |
| Clean stale entries | git_worktree_prune | After manual deletion |
| Unlock before remove | git_worktree_unlock | If worktree is locked |
| Check worktree status | git_worktree_list | Verify before removal |

## Safety Features

- Cannot remove locked worktrees without force=true
- Fails if worktree path doesn't exist
- Removes both working directory and .git/worktrees entry

## Remember

- Verify worktree is no longer needed before removal
- Check if locked with git_worktree_list first
- Use force=true only for stuck/corrupted worktrees
- This is permanent - removed directory cannot be recovered
- Proper removal is better than manual deletion + prune
