---
name: github-get-pull-request-reviews
description: >
  Get all reviews for a pull request showing approvals, change requests, and comments.
  WHEN: User asks "what did reviewers say", "show reviews", "who approved", "review status".
  WHEN NOT: Creating a review (use github_create_pull_request_review), getting PR status (use github_get_pull_request_status).
version: 0.1.0
---

# GitHub Get Pull Request Reviews

## Core Concept

`mcp__plugin_kg_kodegen__github_get_pull_request_reviews` fetches all reviews submitted on a PR, showing who reviewed, their verdict (approved/changes requested/commented), and their feedback.

## Key Parameters

### Required (all)
- **owner**: Repository owner
- **repo**: Repository name
- **pull_number**: Pull request number

## Usage Examples

### Get All Reviews
```json
{
  "owner": "octocat",
  "repo": "hello-world",
  "pull_number": 42
}
```

## Response Fields

Returns `GitHubPrReviewsOutput` with:
- **reviews**: Array of `GitHubReview` objects

Each review contains:
- **id**: Review ID
- **author**: Reviewer username
- **state**: Review verdict (see below)
- **body**: Review comment text
- **submitted_at**: When review was submitted

## Review States

- **APPROVED**: Reviewer approved the changes
- **CHANGES_REQUESTED**: Reviewer wants changes before approval
- **COMMENTED**: Reviewer left comments without approval/blocking
- **DISMISSED**: Review was dismissed (no longer valid)
- **PENDING**: Review in progress, not submitted

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| See reviews | github_get_pull_request_reviews | All review verdicts |
| Submit review | github_create_pull_request_review | Approve/request changes |
| Check merge status | github_get_pull_request_status | Overall PR health |
| Add inline comment | github_add_pull_request_review_comment | Comment on code |

## Remember

- APPROVED allows merge (if no blocking reviews)
- CHANGES_REQUESTED blocks merge
- COMMENTED is non-blocking feedback
- Latest review per user typically counts
- Check branch protection for required reviewers
