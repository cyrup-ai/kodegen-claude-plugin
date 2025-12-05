---
name: github-list-commits
description: >
  List commits in a GitHub repository with filtering options.
  WHEN: User asks "show recent commits", "commit history", "what was committed", "who committed", needs to find a specific commit.
  WHEN NOT: Getting details of a specific commit (use github_get_commit), viewing diffs (use github_get_commit).
version: 0.1.0
---

# GitHub List Commits

## Core Concept

`mcp__plugin_kg_kodegen__github_list_commits` retrieves commit history from a GitHub repository with powerful filtering by branch, path, author, and date range.

## Key Parameters

### Required
- **owner** (string): Repository owner
- **repo** (string): Repository name

### Optional
- **sha** (string): Branch name or commit SHA to start from
- **path** (string): Filter commits that touch this file path
- **author** (string): Filter by author (GitHub login or email)
- **since** (string): Only commits after this date (ISO 8601)
- **until** (string): Only commits before this date (ISO 8601)
- **page** (u32): Page number
- **per_page** (u8): Results per page (max 100)

## Usage Examples

### List recent commits on main
```json
{
  "owner": "rust-lang",
  "repo": "rust"
}
```

### Commits on a specific branch
```json
{
  "owner": "myuser",
  "repo": "my-project",
  "sha": "develop"
}
```

### Commits affecting a specific file
```json
{
  "owner": "facebook",
  "repo": "react",
  "path": "packages/react/src/React.js"
}
```

### Commits by a specific author in date range
```json
{
  "owner": "nodejs",
  "repo": "node",
  "author": "octocat",
  "since": "2024-01-01T00:00:00Z",
  "until": "2024-12-31T23:59:59Z"
}
```

## Output Fields

Each commit includes:
- **sha**: Commit SHA
- **message**: Commit message
- **author_name/email**: Author info
- **date**: Commit date
- **html_url**: GitHub web URL

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| List commits | github_list_commits | History overview |
| Specific commit details | github_get_commit | Full info + diffs |
| Find commit by message | github_list_commits | Search through results |
| File history | github_list_commits with path | Filter by file |

## Remember

- Default returns commits from default branch
- Use `sha` param for specific branch or starting point
- Path filter shows commits that modified that file
- Date format is ISO 8601 (e.g., "2024-01-15T00:00:00Z")
- Pagination needed for long histories
