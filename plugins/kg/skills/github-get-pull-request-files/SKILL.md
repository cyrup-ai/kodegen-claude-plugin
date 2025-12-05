---
name: github-get-pull-request-files
description: >
  Get all files changed in a pull request with diff statistics and patch content.
  WHEN: User asks "what files changed", "show PR diff", "list changed files", "review PR changes".
  WHEN NOT: Getting PR status (use github_get_pull_request_status), getting full diff (use git locally).
version: 0.1.0
---

# GitHub Get Pull Request Files

## Core Concept

`mcp__plugin_kg_kodegen__github_get_pull_request_files` retrieves all files changed in a PR with their status (added/modified/deleted), line counts, and optional patch content.

## Key Parameters

### Required (all)
- **owner**: Repository owner
- **repo**: Repository name
- **pr_number**: Pull request number

## Usage Examples

### Get Changed Files
```json
{
  "owner": "octocat",
  "repo": "hello-world",
  "pr_number": 42
}
```

## Response Fields

Returns `GitHubGetPrFilesOutput` with:
- **count**: Number of files changed
- **files**: Array of `GitHubPrFile` objects

Each file contains:
- **filename**: Path to the file
- **status**: "added", "modified", "removed", "renamed"
- **additions**: Lines added
- **deletions**: Lines deleted
- **changes**: Total changes (additions + deletions)
- **patch**: The actual diff content (if available)

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| List changed files | github_get_pull_request_files | File list with stats |
| Get PR status | github_get_pull_request_status | Merge readiness |
| Get reviews | github_get_pull_request_reviews | Approval status |
| Full local diff | git diff locally | Complete diff analysis |

## Remember

- Returns file paths relative to repo root
- Status values: added, modified, removed, renamed
- Patch field contains actual diff hunks
- Use for code review preparation
- Large PRs may have truncated patches
