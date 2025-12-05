---
name: github-request-copilot-review
description: >
  Request GitHub Copilot to automatically review a pull request (experimental feature).
  WHEN: User wants AI code review, automated review, "copilot review", initial feedback.
  WHEN NOT: Human review (use github_create_pull_request_review), getting existing reviews (use github_get_pull_request_reviews).
version: 0.1.0
---

# GitHub Request Copilot Review

## Core Concept

`mcp__plugin_kg_kodegen__github_request_copilot_review` triggers GitHub Copilot to analyze a PR and provide automated code review feedback. This is an EXPERIMENTAL feature.

## Key Parameters

### Required (all)
- **owner**: Repository owner
- **repo**: Repository name
- **pull_number**: PR number to review

## Usage Examples

### Request Copilot Review
```json
{
  "owner": "octocat",
  "repo": "hello-world",
  "pull_number": 42
}
```

## What Copilot Reviews

- Code quality and best practices
- Potential bugs and issues
- Security vulnerabilities
- Performance improvements
- Code style and conventions

## Workflow

1. Create or update a pull request
2. Request Copilot review with this tool
3. Wait a few moments for analysis
4. Check PR comments: `github_get_pull_request_reviews`
5. Review Copilot's suggestions

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| AI code review | github_request_copilot_review | Automated analysis |
| Human review | github_create_pull_request_review | Manual approval |
| See all reviews | github_get_pull_request_reviews | Check feedback |
| Inline comment | github_add_pull_request_review_comment | Specific line |

## Remember

- This is EXPERIMENTAL - API may change
- Requires Copilot access on the repository
- Review appears as PR comments after delay
- Not a replacement for human review
- Combine with get_pull_request_reviews to see results
