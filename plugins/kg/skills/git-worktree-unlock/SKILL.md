---
name: git-worktree-unlock
description: >
  Unlock a previously locked worktree to allow deletion.
  WHEN: User says "unlock worktree", "allow worktree deletion", "remove worktree lock".
  WHEN NOT: Want to keep locked, locking worktree (use git_worktree_lock).
version: 0.1.0
---

# Git Worktree Unlock

## Core Concept

`mcp__plugin_kg_kodegen__git_worktree_unlock` removes the lock from a worktree, allowing it to be deleted with git_worktree_remove or cleaned with git_worktree_prune.

## Key Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| path | string | Yes | - | Path to the main Git repository |
| worktree_path | string | Yes | - | Path to the worktree to unlock |

## Usage Examples

### Unlock worktree before removal
```json
{
  "path": ".",
  "worktree_path": "../locked-worktree"
}
```

### Unlock worktree on network drive
```json
{
  "path": "/main/repo",
  "worktree_path": "/network/share/worktree"
}
```

## Typical Workflow

1. Check lock status: `git_worktree_list`
2. Unlock if needed: `git_worktree_unlock`
3. Remove worktree: `git_worktree_remove`

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Unlock worktree | git_worktree_unlock | Removes lock |
| Lock worktree | git_worktree_lock | Prevents deletion |
| Check if locked | git_worktree_list | Shows is_locked field |
| Remove after unlock | git_worktree_remove | Clean removal |

## Remember

- Fails if worktree is not currently locked
- Does not delete the worktree, only removes lock
- Use git_worktree_list to verify lock status first
- After unlocking, worktree can be pruned or removed
- Lock and unlock are complementary operations
