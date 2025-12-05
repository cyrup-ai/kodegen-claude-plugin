---
name: git-branch-create
description: >
  Create a new Git branch from HEAD or a specified commit/branch.
  WHEN: User says "create branch", "new branch", "start feature", "branch from", "make a branch for".
  WHEN NOT: Switching to existing branch (use git_checkout), listing branches (use git_branch_list).
version: 0.1.0
---

# Git Branch Create

## Core Concept

`mcp__plugin_kg_kodegen__git_branch_create` creates a new branch in a Git repository. It can create from the current HEAD or from a specified starting point (branch, tag, or commit). Optionally checks out the new branch immediately after creation.

## Key Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| path | string | Yes | - | Path to the Git repository |
| branch | string | Yes | - | Name for the new branch |
| from_branch | string | No | HEAD | Starting point (branch, tag, or commit SHA) |
| force | boolean | No | false | Overwrite if branch already exists |
| checkout | boolean | No | false | Switch to branch after creation |

## Usage Examples

### Create and checkout a feature branch
```json
{
  "path": "/path/to/repo",
  "branch": "feature/user-auth",
  "checkout": true
}
```

### Create branch from specific commit
```json
{
  "path": ".",
  "branch": "hotfix/critical-bug",
  "from_branch": "main",
  "checkout": true
}
```

### Create branch from tag without switching
```json
{
  "path": "/project",
  "branch": "release/v2.0-prep",
  "from_branch": "v1.9.0",
  "checkout": false
}
```

### Force recreate existing branch
```json
{
  "path": ".",
  "branch": "experiment",
  "from_branch": "develop",
  "force": true,
  "checkout": true
}
```

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Create new branch | git_branch_create | Creates branch ref |
| Switch to existing branch | git_checkout | Switches HEAD without creating |
| List branches | git_branch_list | Read-only enumeration |
| Delete branch | git_branch_delete | Removes branch ref |
| Rename branch | git_branch_rename | Changes branch name |

## Branch Naming Conventions

Use prefixes to indicate branch type:
- `feature/` - New features: `feature/oauth-integration`
- `bugfix/` or `fix/` - Bug fixes: `bugfix/null-pointer`
- `hotfix/` - Urgent production fixes: `hotfix/security-patch`
- `release/` - Release preparation: `release/v2.0.0`
- `experimental/` - Experiments: `experimental/new-arch`

Include ticket numbers when applicable: `feature/JIRA-123-user-dashboard`

## Remember

- Use `checkout: true` to immediately switch to the new branch
- Without `from_branch`, creates from current HEAD position
- `force: true` overwrites existing branch - use with caution
- Branch names should use lowercase with hyphens
- Cannot create branch with same name as existing without force flag
