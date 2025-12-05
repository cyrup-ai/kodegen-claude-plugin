---
name: git-branch-list
description: >
  List all local branches in a Git repository.
  WHEN: User asks "what branches exist", "show branches", "list branches", "which branches".
  WHEN NOT: Need remote branches (use terminal with git branch -r), need detailed branch info (use git_log per branch).
version: 0.1.0
---

# Git Branch List

## Core Concept

`mcp__plugin_kg_kodegen__git_branch_list` enumerates all local branches in a Git repository. Returns the branch names, total count, and identifies the currently checked-out branch.

## Key Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| path | string | Yes | - | Path to the Git repository |

## Usage Examples

### List branches in current directory
```json
{
  "path": "."
}
```

### List branches in specific repository
```json
{
  "path": "/home/user/projects/my-app"
}
```

## Output Format

Returns JSON with:
- `success`: boolean indicating operation success
- `branches`: array of branch names
- `count`: total number of branches

Human-readable summary shows:
- Total branch count
- Current branch name (highlighted)

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| List local branches | git_branch_list | Fast enumeration |
| List remote branches | terminal | Use `git branch -r` |
| List all branches | terminal | Use `git branch -a` |
| Get current branch only | git_status | Shows branch + more info |
| Find branches by pattern | terminal | Use `git branch --list 'feature/*'` |

## Remember

- Lists only local branches (not remote tracking branches)
- Highlights the currently checked-out branch
- Works from any subdirectory within the repository
- Returns empty list if path is not a Git repository
- Use before branch operations to discover available branches
