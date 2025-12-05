---
name: github-add-issue-comment
description: >
  Add a comment to an existing GitHub issue. Supports Markdown formatting.
  WHEN: User wants to reply, comment on issue, add feedback, respond to issue.
  WHEN NOT: Creating new issue (use github_create_issue), updating issue metadata (use github_update_issue).
version: 0.1.0
---

# GitHub Add Issue Comment

## Core Concept

`mcp__plugin_kg_kodegen__github_add_issue_comment` adds a new comment to an existing GitHub issue or pull request. Supports full Markdown formatting including code blocks, mentions, and references.

## Key Parameters

### Required (all)
- **owner**: Repository owner
- **repo**: Repository name
- **issue_number**: The issue/PR number
- **body**: Comment text (supports Markdown)

## Usage Examples

### Simple Comment
```json
{
  "owner": "octocat",
  "repo": "hello-world",
  "issue_number": 42,
  "body": "This has been fixed in the latest release."
}
```

### Comment with Markdown
```json
{
  "owner": "octocat",
  "repo": "hello-world",
  "issue_number": 42,
  "body": "Fixed in PR #123\n\n```python\nprint('hello')\n```\n\n@alice please verify this works for you."
}
```

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Add comment | github_add_issue_comment | New comment on issue/PR |
| Create issue | github_create_issue | New issue entirely |
| Update issue | github_update_issue | Change title/body/state |
| PR inline comment | github_add_pull_request_review_comment | Comment on code line |

## Remember

- Creates a NEW comment each time (not idempotent)
- Full Markdown support (headings, code blocks, lists)
- Use @mentions to notify users
- Reference issues/PRs with #number
- Works for both issues and pull requests
