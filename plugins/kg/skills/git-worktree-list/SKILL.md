---
name: git-worktree-list
description: >
  List all worktrees in the repository with their status.
  WHEN: User asks "what worktrees exist", "show worktrees", "list worktrees", "which worktrees".
  WHEN NOT: Looking for branches (use git_branch_list), creating worktree (use git_worktree_add).
version: 0.1.0
---

# Git Worktree List

## Core Concept

`mcp__plugin_kg_kodegen__git_worktree_list` shows all worktrees linked to a repository, including the main worktree and any linked worktrees, with their paths, branches, and lock status.

## Key Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| path | string | Yes | - | Path to any worktree in the repository |

## Usage Examples

### List worktrees from main repository
```json
{
  "path": "."
}
```

### List worktrees from linked worktree
```json
{
  "path": "/path/to/any/worktree"
}
```

## Output Fields

| Field | Description |
|-------|-------------|
| path | Filesystem path to worktree |
| git_dir | Path to .git directory |
| is_main | True if main repository |
| is_bare | True if bare repository |
| head_branch | Checked-out branch name |
| head_commit | Current commit SHA |
| is_locked | True if worktree is locked |
| lock_reason | Reason for lock (if any) |
| is_detached | True if HEAD is detached |

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| List all worktrees | git_worktree_list | Shows all linked worktrees |
| List branches | git_branch_list | Branch enumeration only |
| Check worktree before remove | git_worktree_list | Verify status |
| Find locked worktrees | git_worktree_list | Shows is_locked field |

## Remember

- Works from any worktree in the repository
- Shows main worktree and all linked worktrees
- Check is_locked before attempting removal
- Detached worktrees indicate checkout by commit, not branch
- Run before git_worktree_remove to verify targets
