---
name: github-create-pull-request-review
description: >
  Submit a review on a pull request - approve, request changes, or comment.
  WHEN: User wants to approve PR, request changes, leave review, sign off on changes.
  WHEN NOT: Just adding inline comment (use github_add_pull_request_review_comment), getting reviews (use github_get_pull_request_reviews).
version: 0.1.0
---

# GitHub Create Pull Request Review

## Core Concept

`mcp__plugin_kg_kodegen__github_create_pull_request_review` submits a formal review on a PR. The event type determines the impact: APPROVE allows merge, REQUEST_CHANGES blocks it, COMMENT provides feedback without blocking.

## Key Parameters

### Required
- **owner**: Repository owner
- **repo**: Repository name
- **pull_number**: Pull request number
- **event**: Review action - "APPROVE", "REQUEST_CHANGES", or "COMMENT"

### Optional
- **body**: Overall review comment (recommended)
- **commit_id**: Specific commit SHA to review (defaults to latest)

## Usage Examples

### Approve PR
```json
{
  "owner": "octocat",
  "repo": "hello-world",
  "pull_number": 42,
  "event": "APPROVE",
  "body": "LGTM! Great work on the refactoring."
}
```

### Request Changes
```json
{
  "owner": "octocat",
  "repo": "hello-world",
  "pull_number": 42,
  "event": "REQUEST_CHANGES",
  "body": "Please address the security concerns before merging."
}
```

### Comment Only (Non-blocking)
```json
{
  "owner": "octocat",
  "repo": "hello-world",
  "pull_number": 42,
  "event": "COMMENT",
  "body": "Some suggestions for improvement, but not blocking."
}
```

## Event Types Explained

| Event | Effect | Use Case |
|-------|--------|----------|
| APPROVE | Allows merge | PR is good to go |
| REQUEST_CHANGES | Blocks merge | Must fix issues first |
| COMMENT | No effect on merge | Suggestions, questions |

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Submit review verdict | github_create_pull_request_review | Approve/block/comment |
| Inline code comment | github_add_pull_request_review_comment | Comment on specific line |
| See existing reviews | github_get_pull_request_reviews | Check approval status |
| Copilot review | github_request_copilot_review | Automated AI review |

## Remember

- APPROVE can trigger merge if all requirements met
- REQUEST_CHANGES blocks until you re-review
- Use COMMENT for suggestions that don't block
- Include descriptive body explaining your review
- Multiple reviews can be submitted on same PR
