---
name: git-worktree-lock
description: >
  Lock a worktree to prevent accidental deletion or pruning.
  WHEN: User says "lock worktree", "protect worktree", "prevent worktree deletion", "worktree on USB".
  WHEN NOT: Done with worktree (use git_worktree_remove), unlocking (use git_worktree_unlock).
version: 0.1.0
---

# Git Worktree Lock

## Core Concept

`mcp__plugin_kg_kodegen__git_worktree_lock` prevents a worktree from being deleted by git_worktree_remove or cleaned by git_worktree_prune. Essential for worktrees on removable media or network drives.

## Key Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| path | string | Yes | - | Path to the main Git repository |
| worktree_path | string | Yes | - | Path to the worktree to lock |
| reason | string | No | - | Optional reason for the lock |

## Usage Examples

### Lock worktree on USB drive
```json
{
  "path": ".",
  "worktree_path": "/mnt/usb/my-feature",
  "reason": "On removable USB drive"
}
```

### Lock worktree on network share
```json
{
  "path": "/main/repo",
  "worktree_path": "/network/share/worktree",
  "reason": "Network drive - may be unmounted"
}
```

### Lock without reason
```json
{
  "path": ".",
  "worktree_path": "../important-work"
}
```

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Protect worktree | git_worktree_lock | Prevents deletion |
| Allow deletion | git_worktree_unlock | Removes lock |
| Delete worktree | git_worktree_remove | After unlocking |
| Check lock status | git_worktree_list | Shows is_locked field |

## Use Cases

- Worktrees on removable media (USB, external drives)
- Worktrees on network shares that may disconnect
- Protecting active work during cleanup operations
- Preventing accidental deletion in shared environments

## Remember

- Lock reason is optional but helps document purpose
- Cannot lock already-locked worktree (unlock first)
- Lock persists until explicitly unlocked
- git_worktree_remove fails on locked worktrees (without force)
- Use git_worktree_list to check current lock status
