---
name: github-get-issue
description: >
  Fetch a single GitHub issue by number with full details including title, body, state, labels, assignees, and comments count.
  WHEN: User asks about issue #X, needs issue details, wants to see issue content, check issue status.
  WHEN NOT: Listing multiple issues (use github_list_issues), searching issues (use github_search_issues).
version: 0.1.0
---

# GitHub Get Issue

## Core Concept

`mcp__plugin_kg_kodegen__github_get_issue` fetches detailed information about a specific GitHub issue by its number. Works for both issues AND pull requests (GitHub treats PRs as issues internally).

## Key Parameters

### Required (all)
- **owner**: Repository owner (user or organization name)
- **repo**: Repository name
- **issue_number**: The issue NUMBER (e.g., 42 from #42), not the internal ID

## Usage Examples

### Get Issue Details
```json
{
  "owner": "octocat",
  "repo": "hello-world",
  "issue_number": 42
}
```

## Response Fields

The response includes a `GitHubIssue` object with:
- **number**: Issue number
- **title**: Issue title
- **body**: Issue description (Markdown)
- **state**: "open" or "closed"
- **author**: Issue creator username
- **labels**: Array of label names
- **assignees**: Array of assigned usernames
- **comments_count**: Number of comments
- **created_at, updated_at**: ISO timestamps
- **closed_at**: When closed (if applicable)
- **html_url**: Link to issue on GitHub.com

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Get single issue | github_get_issue | Full details for one issue |
| List many issues | github_list_issues | Multiple issues with filtering |
| Get issue comments | github_get_issue_comments | See discussion thread |
| Search across repos | github_search_issues | Find issues by keyword |

## Remember

- Works for BOTH issues and pull requests
- Use issue NUMBER not internal ID
- Read-only operation (idempotent)
- Returns full issue details including body content
- Check state field to see if open/closed
