---
name: git-worktree-add
description: >
  Create a new Git worktree for parallel development on multiple branches.
  WHEN: User says "create worktree", "add worktree", "work on multiple branches", "parallel development", "new worktree".
  WHEN NOT: Just switching branches (use git_checkout), creating branch only (use git_branch_create).
version: 0.1.0
---

# Git Worktree Add

## Core Concept

`mcp__plugin_kg_kodegen__git_worktree_add` creates a new worktree linked to the repository. Worktrees allow working on multiple branches simultaneously in separate directories, sharing the same Git database.

## Key Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| path | string | Yes | - | Path to the main Git repository |
| worktree_path | string | Yes | - | Path where new worktree will be created |
| branch | string | No | HEAD | Branch, tag, or commit to checkout |
| force | boolean | No | false | Force creation if path exists |

## Usage Examples

### Create worktree for feature branch
```json
{
  "path": ".",
  "worktree_path": "../feature-work",
  "branch": "feature/new-feature"
}
```

### Create worktree tracking remote branch
```json
{
  "path": ".",
  "worktree_path": "../upstream-sync",
  "branch": "origin/main"
}
```

### Create worktree at specific commit
```json
{
  "path": "/main-repo",
  "worktree_path": "/hotfix-work",
  "branch": "v1.0.0"
}
```

### Force recreate existing worktree path
```json
{
  "path": ".",
  "worktree_path": "../experiment",
  "branch": "develop",
  "force": true
}
```

## Why Use Worktrees?

- Work on hotfix while feature branch has uncommitted changes
- Run tests on one branch while developing on another
- Compare implementations across branches side-by-side
- Avoid stash/unstash cycles when context switching

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Work on multiple branches | git_worktree_add | Parallel directories |
| Switch single directory | git_checkout | Simpler, one branch at a time |
| List existing worktrees | git_worktree_list | See all worktrees |
| Remove worktree | git_worktree_remove | Clean up when done |

## Remember

- Worktree path must not already exist (unless force=true)
- Each worktree has its own working directory but shares .git database
- Cannot checkout same branch in multiple worktrees simultaneously
- Use git_worktree_lock to protect worktrees on removable media
- Remove worktrees with git_worktree_remove when finished
