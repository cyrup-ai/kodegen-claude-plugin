---
name: github-get-issue-comments
description: >
  Fetch all comments for a GitHub issue in chronological order.
  WHEN: User wants to see discussion, read comments on issue, review conversation history.
  WHEN NOT: Adding a comment (use github_add_issue_comment), getting issue details (use github_get_issue).
version: 0.1.0
---

# GitHub Get Issue Comments

## Core Concept

`mcp__plugin_kg_kodegen__github_get_issue_comments` retrieves all comments on a GitHub issue in chronological order (oldest first). Works for both issues and pull requests.

## Key Parameters

### Required (all)
- **owner**: Repository owner
- **repo**: Repository name
- **issue_number**: The issue/PR number

## Usage Examples

### Get All Comments
```json
{
  "owner": "octocat",
  "repo": "hello-world",
  "issue_number": 42
}
```

## Response Fields

Returns `GitHubGetIssueCommentsOutput` with:
- **count**: Number of comments
- **comments**: Array of `GitHubComment` objects

Each comment contains:
- **id**: Comment ID
- **author**: Comment author username
- **body**: Comment text (Markdown)
- **created_at**: When comment was created
- **updated_at**: When comment was last edited

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Read comments | github_get_issue_comments | See full discussion |
| Add comment | github_add_issue_comment | Post new comment |
| Get issue itself | github_get_issue | Issue metadata and body |
| Get PR reviews | github_get_pull_request_reviews | Review approvals/changes |

## Remember

- Comments returned in chronological order (oldest first)
- Works for both issues and pull requests
- Read-only operation (idempotent)
- Use created_at to determine comment age
- Comment body supports Markdown
