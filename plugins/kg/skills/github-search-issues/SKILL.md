---
name: github-search-issues
description: >
  Search for issues and pull requests across GitHub using powerful search syntax.
  WHEN: User wants to find issues, search by keyword, "find issues about X", cross-repo search.
  WHEN NOT: Listing issues in one repo (use github_list_issues), getting specific issue (use github_get_issue).
version: 0.1.0
---

# GitHub Search Issues

## Core Concept

`mcp__plugin_kg_kodegen__github_search_issues` uses GitHub's search syntax to find issues and PRs across repositories. Supports filtering by repo, state, labels, assignee, author, dates, and more.

## Key Parameters

### Required
- **query**: GitHub search query string

### Optional
- **sort**: Sort by "created", "updated", "comments" (default: best match)
- **order**: "asc" or "desc" (default: desc)
- **page**: Page number (default: 1)
- **per_page**: Results per page, max 100 (default: 30)

## Usage Examples

### Search in Specific Repo
```json
{
  "query": "repo:octocat/hello-world is:open"
}
```

### Search by Labels
```json
{
  "query": "repo:octocat/hello-world label:bug label:priority-high"
}
```

### Search by Author and Date
```json
{
  "query": "repo:octocat/hello-world author:alice created:>=2024-01-01"
}
```

### Combined Search with Sorting
```json
{
  "query": "repo:octocat/hello-world is:open label:bug assignee:alice",
  "sort": "created",
  "order": "desc"
}
```

### Cross-Repo Search
```json
{
  "query": "org:github is:issue is:open authentication"
}
```

## Search Query Syntax

| Qualifier | Example | Description |
|-----------|---------|-------------|
| repo: | repo:owner/name | Specific repository |
| is: | is:open, is:issue, is:pr | State and type |
| label: | label:bug | Has label |
| author: | author:username | Created by |
| assignee: | assignee:username | Assigned to |
| created: | created:>=2024-01-01 | Creation date |
| updated: | updated:<=2024-06-01 | Last update |

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Cross-repo search | github_search_issues | Full search syntax |
| List in one repo | github_list_issues | Simpler, faster |
| Get single issue | github_get_issue | Full details |
| Get comments | github_get_issue_comments | Discussion thread |

## Remember

- Search API has stricter rate limits (30 req/min)
- Combines issues AND pull requests
- Use repo: qualifier for specific repo
- Use is:issue or is:pr to filter type
- Query syntax matches GitHub web search
