---
name: github-create-issue
description: >
  Create a new issue in a GitHub repository with title, body, labels, and assignees.
  WHEN: User wants to report a bug, request a feature, create an issue, file a ticket, open an issue.
  WHEN NOT: Creating a pull request (use github_create_pull_request), commenting on existing issue (use github_add_issue_comment).
version: 0.1.0
---

# GitHub Create Issue

## Core Concept

`mcp__plugin_kg_kodegen__github_create_issue` creates a new issue in a GitHub repository. Supports setting title, body, labels, and assignees. Returns the created issue number and URL.

## Key Parameters

### Required
- **owner**: Repository owner (user or organization name)
- **repo**: Repository name
- **title**: Issue title

### Optional
- **body**: Issue description (supports Markdown)
- **labels**: Array of label names (labels must exist in repo)
- **assignees**: Array of usernames to assign (must be collaborators)

## Usage Examples

### Basic Issue
```json
{
  "owner": "octocat",
  "repo": "hello-world",
  "title": "Bug: Login button not working"
}
```

### Full Issue with Labels and Assignees
```json
{
  "owner": "octocat",
  "repo": "hello-world",
  "title": "Bug: Login fails on mobile",
  "body": "## Description\nWhen I try to login on mobile, the form doesn't submit.\n\n## Steps to Reproduce\n1. Open site on mobile\n2. Click login\n3. Enter credentials\n4. Nothing happens",
  "labels": ["bug", "priority-high", "mobile"],
  "assignees": ["octocat", "alice"]
}
```

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Create new issue | github_create_issue | Creates issue with metadata |
| Add comment to issue | github_add_issue_comment | Adds to existing issue |
| Create pull request | github_create_pull_request | For code changes |
| Update existing issue | github_update_issue | Modify title/body/state |

## Remember

- Body supports full Markdown formatting (code blocks, lists, images)
- Labels must already exist in the repository
- Assignees must be collaborators on the repository
- Each call creates a NEW issue (not idempotent)
- Use @mentions in body to notify users
