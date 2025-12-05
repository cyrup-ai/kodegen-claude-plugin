---
name: git-branch-rename
description: >
  Rename a Git branch to a new name.
  WHEN: User says "rename branch", "change branch name", "fix branch name", "branch typo".
  WHEN NOT: Creating new branch (use git_branch_create), copying branch (create new from existing).
version: 0.1.0
---

# Git Branch Rename

## Core Concept

`mcp__plugin_kg_kodegen__git_branch_rename` renames an existing branch. Automatically updates HEAD if renaming the currently checked-out branch.

## Key Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| path | string | Yes | - | Path to the Git repository |
| old_name | string | Yes | - | Current branch name |
| new_name | string | Yes | - | New branch name |
| force | boolean | No | false | Overwrite if new_name already exists |

## Usage Examples

### Fix typo in branch name
```json
{
  "path": ".",
  "old_name": "feature/user-athentication",
  "new_name": "feature/user-authentication"
}
```

### Standardize naming convention
```json
{
  "path": "/project",
  "old_name": "newFeature",
  "new_name": "feature/new-feature"
}
```

### Force rename (overwrite existing)
```json
{
  "path": ".",
  "old_name": "temp-branch",
  "new_name": "feature/main-work",
  "force": true
}
```

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Rename branch | git_branch_rename | Changes name in place |
| Create branch copy | git_branch_create | Creates from existing branch |
| Delete old, create new | git_branch_delete + git_branch_create | Two-step alternative |
| Check branch exists | git_branch_list | Verify before renaming |

## Remember

- Automatically updates HEAD if renaming current branch
- Use `force: true` only to overwrite existing branches
- Verify `old_name` exists with git_branch_list first
- Remote tracking is not automatically updated
- Inform team members after renaming shared branches
