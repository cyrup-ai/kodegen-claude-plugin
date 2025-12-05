---
name: github-list-issues
description: >
  List and filter issues in a GitHub repository by state, labels, and pagination.
  WHEN: User asks "what issues exist", "show open bugs", "list my issues", "show issues in repo".
  WHEN NOT: Searching across repos (use github_search_issues), getting single issue (use github_get_issue).
version: 0.1.0
---

# GitHub List Issues

## Core Concept

`mcp__plugin_kg_kodegen__github_list_issues` lists issues in a specific repository with filtering by state and labels. Returns summaries, not full issue bodies.

## Key Parameters

### Required
- **owner**: Repository owner
- **repo**: Repository name

### Optional
- **state**: "open", "closed", or "all" (default: "open")
- **labels**: Array of label names (AND logic - must have ALL labels)
- **page**: Page number for pagination (default: 1)
- **per_page**: Results per page, max 100 (default: 30)

## Usage Examples

### List Open Issues
```json
{
  "owner": "octocat",
  "repo": "hello-world"
}
```

### List Closed Issues
```json
{
  "owner": "octocat",
  "repo": "hello-world",
  "state": "closed"
}
```

### Filter by Labels
```json
{
  "owner": "octocat",
  "repo": "hello-world",
  "labels": ["bug", "priority-high"]
}
```

### With Pagination
```json
{
  "owner": "octocat",
  "repo": "hello-world",
  "per_page": 50,
  "page": 2
}
```

## Response Fields

Returns `GitHubListIssuesOutput` with:
- **count**: Number of issues returned
- **issues**: Array of `GitHubIssueSummary` objects

Each summary contains: number, title, state, author, created_at, labels

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| List issues in repo | github_list_issues | Filter by state/labels |
| Search across repos | github_search_issues | Cross-repo keyword search |
| Get single issue | github_get_issue | Full details for one |
| Get issue comments | github_get_issue_comments | Discussion thread |

## Remember

- Default state is "open" (not "all")
- Labels filter uses AND logic (must match ALL)
- Returns summaries, not full bodies
- Use pagination for large repositories
- Max 100 results per page
