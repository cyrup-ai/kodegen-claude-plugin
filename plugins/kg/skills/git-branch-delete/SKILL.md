---
name: git-branch-delete
description: >
  Delete a local Git branch from the repository.
  WHEN: User says "delete branch", "remove branch", "clean up branches", "drop branch".
  WHEN NOT: Deleting remote branch (use terminal with git push --delete), renaming branch (use git_branch_rename).
version: 0.1.0
---

# Git Branch Delete

## Core Concept

`mcp__plugin_kg_kodegen__git_branch_delete` removes a local branch from the Git repository. By default, it only deletes fully merged branches. Use the force flag to delete unmerged branches.

## Key Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| path | string | Yes | - | Path to the Git repository |
| branch | string | Yes | - | Name of branch to delete |
| force | boolean | No | false | Force delete even if not fully merged |

## Usage Examples

### Delete a merged feature branch
```json
{
  "path": ".",
  "branch": "feature/completed-feature"
}
```

### Force delete an unmerged branch
```json
{
  "path": "/path/to/repo",
  "branch": "experiment/abandoned",
  "force": true
}
```

### Clean up old branch
```json
{
  "path": ".",
  "branch": "old-feature-branch",
  "force": false
}
```

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Delete local branch | git_branch_delete | Removes local ref |
| Delete remote branch | terminal | Use `git push origin --delete branch-name` |
| List branches first | git_branch_list | See what exists before deleting |
| Check current branch | git_status | Ensure not on branch to delete |

## Safety Features

- Cannot delete the currently checked-out branch (switch first)
- Without `force: true`, only merged branches can be deleted
- With `force: true`, deletes regardless of merge status

## Remember

- Always verify branch is no longer needed before deletion
- Cannot delete branch you are currently on
- Use `force: true` only when certain the work is disposable
- This removes the local branch only, not remote tracking branches
- Deleted branches can be recovered if commits still exist on remote
