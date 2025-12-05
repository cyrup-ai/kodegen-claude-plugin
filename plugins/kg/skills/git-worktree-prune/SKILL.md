---
name: git-worktree-prune
description: >
  Remove stale worktree administrative files for deleted worktree directories.
  WHEN: User says "prune worktrees", "clean up worktrees", "remove stale worktrees", "worktree cleanup".
  WHEN NOT: Removing active worktree (use git_worktree_remove), listing worktrees (use git_worktree_list).
version: 0.1.0
---

# Git Worktree Prune

## Core Concept

`mcp__plugin_kg_kodegen__git_worktree_prune` removes stale administrative files from .git/worktrees/ for worktrees whose directories have been manually deleted. Safe cleanup operation.

## Key Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| path | string | Yes | - | Path to the Git repository |

## Usage Examples

### Prune stale worktree entries
```json
{
  "path": "."
}
```

### Prune after manual directory deletion
```json
{
  "path": "/home/user/main-repo"
}
```

## When to Use

1. After manually deleting a worktree directory (rm -rf instead of git worktree remove)
2. When worktree directory became inaccessible or corrupted
3. During repository maintenance to clean stale metadata
4. When git_worktree_list shows entries that no longer exist

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Clean stale entries | git_worktree_prune | Removes orphan admin files |
| Remove active worktree | git_worktree_remove | Proper removal process |
| List worktrees | git_worktree_list | Check before pruning |
| Protect worktree | git_worktree_lock | Prevents pruning |

## Remember

- Safe to call multiple times (idempotent)
- Only removes admin files for non-existent directories
- Does not delete actual worktree directories
- Locked worktrees are not pruned
- Use git_worktree_remove for proper worktree cleanup
