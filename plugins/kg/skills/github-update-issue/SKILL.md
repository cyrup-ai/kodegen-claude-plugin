---
name: github-update-issue
description: >
  Update an existing GitHub issue - title, body, state, labels, or assignees.
  WHEN: User wants to close issue, change labels, update description, assign users, reopen issue.
  WHEN NOT: Adding comment (use github_add_issue_comment), creating issue (use github_create_issue).
version: 0.1.0
---

# GitHub Update Issue

## Core Concept

`mcp__plugin_kg_kodegen__github_update_issue` modifies an existing issue. Supports partial updates - only specified fields are changed. Can close/reopen issues, change labels, and reassign.

## Key Parameters

### Required
- **owner**: Repository owner
- **repo**: Repository name
- **issue_number**: Issue number to update

### Optional (specify only what to change)
- **title**: New title
- **body**: New description (replaces existing)
- **state**: "open" or "closed"
- **labels**: Array of labels (REPLACES all existing labels)
- **assignees**: Array of usernames (REPLACES all existing assignees)

## Usage Examples

### Close an Issue
```json
{
  "owner": "octocat",
  "repo": "hello-world",
  "issue_number": 42,
  "state": "closed"
}
```

### Update Labels (Replaces All)
```json
{
  "owner": "octocat",
  "repo": "hello-world",
  "issue_number": 42,
  "labels": ["bug", "resolved", "v2.0"]
}
```

### Update Multiple Fields
```json
{
  "owner": "octocat",
  "repo": "hello-world",
  "issue_number": 42,
  "title": "Updated: Bug in login flow",
  "body": "Revised description with more details...",
  "assignees": ["alice", "bob"]
}
```

### Clear All Labels
```json
{
  "owner": "octocat",
  "repo": "hello-world",
  "issue_number": 42,
  "labels": []
}
```

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Close/reopen issue | github_update_issue | Change state field |
| Change labels | github_update_issue | Update labels array |
| Add comment | github_add_issue_comment | Doesn't modify issue |
| Get issue first | github_get_issue | See current state |

## Remember

- Labels array REPLACES all existing labels (not additive)
- Assignees array REPLACES all existing assignees (not additive)
- To clear labels/assignees, pass empty array: []
- Only specified fields are updated
- State values: "open" or "closed"
