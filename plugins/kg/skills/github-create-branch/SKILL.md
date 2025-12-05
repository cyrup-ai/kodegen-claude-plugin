---
name: github-create-branch
description: >
  Create a new branch on a GitHub repository (remote operation via API).
  WHEN: User wants to create a remote branch on GitHub directly, says "create branch on GitHub", needs to branch without a local clone.
  WHEN NOT: Creating a local branch (use git_branch_create or terminal), working with a cloned repository locally.
version: 0.1.0
---

# GitHub Create Branch

## Core Concept

`mcp__plugin_kg_kodegen__github_create_branch` creates a new branch directly on GitHub via the API. This is a REMOTE operation - it does not require a local clone of the repository.

## Key Parameters

### Required
- **owner** (string): Repository owner
- **repo** (string): Repository name
- **branch_name** (string): New branch name to create
- **sha** (string): Commit SHA to create branch from (use HEAD commit SHA or specific commit)

## Usage Examples

### Create a feature branch from main's HEAD
```json
{
  "owner": "myuser",
  "repo": "my-project",
  "branch_name": "feature/new-feature",
  "sha": "abc123def456789..."
}
```

### Create a release branch from a specific commit
```json
{
  "owner": "myorg",
  "repo": "product",
  "branch_name": "release/v2.0",
  "sha": "def789abc123456..."
}
```

## Getting the SHA

To get the SHA for the branch point:
1. Use `github_list_commits` to get recent commits
2. Use `github_get_commit` to get a specific commit's SHA
3. Use the SHA from a branch's HEAD via `github_list_branches`

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Create remote branch | github_create_branch | GitHub API, no local clone needed |
| Create local branch | git_branch_create | Local git operation |
| List remote branches | github_list_branches | GitHub API |
| List local branches | git_branch_list | Local git operation |

## Remember

- This is a REMOTE operation via GitHub API
- Requires the exact SHA to branch from
- Does NOT update any local clone
- Use git_branch_create for local branch operations
- Branch name should not include refs/heads/ prefix
