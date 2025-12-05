---
name: github-merge-pull-request
description: >
  Merge a pull request using merge commit, squash, or rebase strategy.
  WHEN: User wants to merge PR, complete PR, "merge this", land changes.
  WHEN NOT: Closing without merge (use github_update_pull_request), checking status (use github_get_pull_request_status).
version: 0.1.0
---

# GitHub Merge Pull Request

## Core Concept

`mcp__plugin_kg_kodegen__github_merge_pull_request` merges a PR into its base branch. Supports three merge strategies: merge commit, squash, and rebase. This is a DESTRUCTIVE operation.

## Key Parameters

### Required
- **owner**: Repository owner
- **repo**: Repository name
- **pr_number**: PR number to merge

### Optional
- **merge_method**: "merge" (default), "squash", or "rebase"
- **commit_title**: Custom merge commit title (for merge/squash)
- **commit_message**: Custom merge commit message
- **sha**: Expected HEAD SHA (prevents race conditions)

## Usage Examples

### Basic Merge
```json
{
  "owner": "octocat",
  "repo": "hello-world",
  "pr_number": 42
}
```

### Squash Merge with Custom Title
```json
{
  "owner": "octocat",
  "repo": "hello-world",
  "pr_number": 42,
  "merge_method": "squash",
  "commit_title": "Add authentication feature (#42)"
}
```

### Rebase Merge
```json
{
  "owner": "octocat",
  "repo": "hello-world",
  "pr_number": 42,
  "merge_method": "rebase"
}
```

### Safe Merge with SHA Check
```json
{
  "owner": "octocat",
  "repo": "hello-world",
  "pr_number": 42,
  "sha": "abc123def456..."
}
```

## Merge Methods Explained

| Method | Effect | Use Case |
|--------|--------|----------|
| merge | Creates merge commit | Preserve all commits |
| squash | Combines into one commit | Clean history |
| rebase | Replays commits on base | Linear history |

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Merge PR | github_merge_pull_request | Land the changes |
| Check before merge | github_get_pull_request_status | Verify ready |
| Close without merge | github_update_pull_request | Set state=closed |
| Get reviews first | github_get_pull_request_reviews | Check approvals |

## Remember

- This is DESTRUCTIVE - cannot easily undo
- Check status BEFORE merging
- Use SHA parameter to prevent race conditions
- squash creates cleaner history
- rebase maintains linear history but rewrites commits
