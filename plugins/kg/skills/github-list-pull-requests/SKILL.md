---
name: github-list-pull-requests
description: >
  List and filter pull requests in a GitHub repository by state and labels.
  WHEN: User asks "show PRs", "what's open", "list pull requests", "my PRs".
  WHEN NOT: Getting specific PR details (use github_get_pull_request_status), searching issues (use github_search_issues).
version: 0.1.0
---

# GitHub List Pull Requests

## Core Concept

`mcp__plugin_kg_kodegen__github_list_pull_requests` lists pull requests in a repository with filtering options. Returns PR summaries including branch information and draft status.

## Key Parameters

### Required
- **owner**: Repository owner
- **repo**: Repository name

### Optional
- **state**: "open", "closed", or "all" (default: "all")
- **labels**: Array of label names to filter by
- **page**: Page number (default: 1)
- **per_page**: Results per page, max 100 (default: 30)

## Usage Examples

### List All Open PRs
```json
{
  "owner": "octocat",
  "repo": "hello-world",
  "state": "open"
}
```

### List PRs with Labels
```json
{
  "owner": "octocat",
  "repo": "hello-world",
  "labels": ["needs-review"]
}
```

### Paginated Results
```json
{
  "owner": "octocat",
  "repo": "hello-world",
  "per_page": 50,
  "page": 2
}
```

## Response Fields

Returns `GitHubListPrsOutput` with:
- **count**: Number of PRs returned
- **pull_requests**: Array of `GitHubPrSummary` objects

Each summary contains:
- number, title, state, author
- **head_ref**: Source branch
- **base_ref**: Target branch
- **draft**: Boolean indicating draft status
- created_at

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| List PRs in repo | github_list_pull_requests | Filter by state/labels |
| Get PR status | github_get_pull_request_status | Merge readiness, checks |
| Get PR files | github_get_pull_request_files | Changed files list |
| Search issues/PRs | github_search_issues | Cross-repo search |

## Remember

- Default state is "all" (includes open and closed)
- Returns summaries with branch info
- Draft PRs are included in results
- Use pagination for repos with many PRs
- Check draft field to filter WIP PRs
