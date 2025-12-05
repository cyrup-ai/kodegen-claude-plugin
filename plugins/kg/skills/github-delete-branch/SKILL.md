---
name: github-delete-branch
description: >
  Delete a branch from a GitHub repository (remote operation via API).
  WHEN: User wants to delete a remote branch on GitHub, clean up merged branches, says "delete branch on GitHub", "remove remote branch".
  WHEN NOT: Deleting a local branch (use git_branch_delete or terminal), pruning local tracking branches.
version: 0.1.0
---

# GitHub Delete Branch

## Core Concept

`mcp__plugin_kg_kodegen__github_delete_branch` deletes a branch directly on GitHub via the API. This is a REMOTE operation - it removes the branch from GitHub, not from any local clone.

## Key Parameters

### Required
- **owner** (string): Repository owner
- **repo** (string): Repository name
- **branch_name** (string): Branch name to delete

## Usage Examples

### Delete a feature branch after merging
```json
{
  "owner": "myuser",
  "repo": "my-project",
  "branch_name": "feature/completed-feature"
}
```

### Delete an old release branch
```json
{
  "owner": "myorg",
  "repo": "product",
  "branch_name": "release/v1.0-old"
}
```

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Delete remote branch | github_delete_branch | GitHub API, removes from GitHub |
| Delete local branch | git_branch_delete | Local git operation |
| List branches first | github_list_branches | See what exists |

## Remember

- This is a REMOTE operation - deletes from GitHub
- Cannot delete the default branch
- Cannot delete protected branches (unless you have admin rights)
- Does NOT affect local branches
- Operation is DESTRUCTIVE - branch is permanently deleted
- Use git push origin --delete for local git equivalent
