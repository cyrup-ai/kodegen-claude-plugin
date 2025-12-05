---
name: github-add-pull-request-review-comment
description: >
  Add an inline review comment to a pull request on specific lines of code.
  WHEN: User wants to comment on specific line, inline review comment, code-level feedback.
  WHEN NOT: Full review submission (use github_create_pull_request_review), general comment (use github_add_issue_comment).
version: 0.1.0
---

# GitHub Add Pull Request Review Comment

## Core Concept

`mcp__plugin_kg_kodegen__github_add_pull_request_review_comment` adds inline comments on specific lines of code in a PR. Supports single-line, multi-line, and threaded comments.

## Key Parameters

### For New Comment (Required)
- **owner**: Repository owner
- **repo**: Repository name
- **pull_number**: PR number
- **body**: Comment text (supports Markdown)
- **commit_id**: Commit SHA being commented on
- **path**: File path relative to repo root
- **line**: Line number in the diff

### Optional
- **side**: "RIGHT" (new code) or "LEFT" (old code) - default: RIGHT
- **start_line**: For multi-line comments, the starting line
- **start_side**: Side of start_line

### For Reply to Existing Comment
- **in_reply_to**: Comment ID to reply to (inherits position)

## Usage Examples

### Single Line Comment
```json
{
  "owner": "octocat",
  "repo": "hello-world",
  "pull_number": 42,
  "body": "Consider using const here instead of let",
  "commit_id": "abc123def456...",
  "path": "src/main.rs",
  "line": 45,
  "side": "RIGHT"
}
```

### Multi-Line Comment (Lines 20-25)
```json
{
  "owner": "octocat",
  "repo": "hello-world",
  "pull_number": 42,
  "body": "This entire function could be simplified",
  "commit_id": "abc123def456...",
  "path": "src/utils.rs",
  "start_line": 20,
  "line": 25,
  "side": "RIGHT"
}
```

### Reply to Existing Comment
```json
{
  "owner": "octocat",
  "repo": "hello-world",
  "pull_number": 42,
  "body": "Good catch! I'll fix that.",
  "in_reply_to": 123456789
}
```

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Inline code comment | github_add_pull_request_review_comment | Specific line feedback |
| Submit review verdict | github_create_pull_request_review | Approve/request changes |
| General PR comment | github_add_issue_comment | Non-code discussion |
| See existing reviews | github_get_pull_request_reviews | Review history |

## Remember

- side: RIGHT for new/changed code, LEFT for old code
- Multi-line: start_line to line (inclusive)
- Reply only needs in_reply_to (inherits position)
- commit_id must be part of the PR
- Body supports Markdown with code blocks
