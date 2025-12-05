---
name: git-status
description: >
  Show repository status including branch, commits, and working directory state.
  WHEN: User asks "what changed", "git status", "current branch", "uncommitted changes", "repository state".
  WHEN NOT: Need detailed diff (use git_diff), need file list (use fs_list_directory).
version: 0.1.0
---

# Git Status

## Core Concept

`mcp__plugin_kg_kodegen__git_status` shows the current state of a Git repository: current branch, commit, upstream tracking, ahead/behind counts, and whether the working directory is clean or has uncommitted changes.

## Key Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| path | string | Yes | - | Path to the Git repository |

## Usage Examples

### Check status of current directory
```json
{
  "path": "."
}
```

### Check status of specific repository
```json
{
  "path": "/home/user/projects/my-app"
}
```

## Output Fields

| Field | Description |
|-------|-------------|
| branch | Current branch name (or "detached HEAD") |
| commit | Full commit SHA of HEAD |
| upstream | Remote tracking branch (if configured) |
| ahead | Commits ahead of upstream |
| behind | Commits behind upstream |
| is_clean | True if no uncommitted changes |
| is_detached | True if HEAD is detached |

## Interpreting Results

- **Clean**: All changes committed, safe to switch branches
- **Dirty**: Uncommitted changes exist, commit or stash before switching
- **Ahead**: Local commits not yet pushed
- **Behind**: Remote has commits not yet pulled
- **Detached**: Not on a branch, create branch to save work

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Check repo state | git_status | Quick overview |
| See what changed | git_diff | Detailed changes |
| List branches | git_branch_list | All branches |
| Commit changes | git_commit | After checking status |

## Remember

- Run before committing to verify what will be included
- Check before pushing to see ahead count
- Check before pulling to see behind count
- Detached HEAD means not on a branch
- Clean status required for some operations
