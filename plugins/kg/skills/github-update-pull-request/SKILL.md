---
name: github-update-pull-request
description: >
  Update a pull request's title, body, state, or base branch.
  WHEN: User wants to edit PR, close/reopen PR, change description, retarget base branch.
  WHEN NOT: Merging PR (use github_merge_pull_request), adding comment (use github_add_issue_comment).
version: 0.1.0
---

# GitHub Update Pull Request

## Core Concept

`mcp__plugin_kg_kodegen__github_update_pull_request` modifies an existing PR. Supports partial updates - only specified fields are changed. Can close/reopen PRs and change the target branch.

## Key Parameters

### Required
- **owner**: Repository owner
- **repo**: Repository name
- **pr_number**: PR number to update

### Optional (specify only what to change)
- **title**: New PR title
- **body**: New description (replaces existing)
- **state**: "open" or "closed"
- **base**: New target branch
- **maintainer_can_modify**: Allow maintainer pushes

## Usage Examples

### Close a PR
```json
{
  "owner": "octocat",
  "repo": "hello-world",
  "pr_number": 42,
  "state": "closed"
}
```

### Update Title and Description
```json
{
  "owner": "octocat",
  "repo": "hello-world",
  "pr_number": 42,
  "title": "Updated: Add new feature with tests",
  "body": "Revised description with more context..."
}
```

### Change Base Branch
```json
{
  "owner": "octocat",
  "repo": "hello-world",
  "pr_number": 42,
  "base": "develop"
}
```

### Reopen a Closed PR
```json
{
  "owner": "octocat",
  "repo": "hello-world",
  "pr_number": 42,
  "state": "open"
}
```

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Edit PR metadata | github_update_pull_request | Change title/body/state |
| Merge PR | github_merge_pull_request | Complete the PR |
| Add comment | github_add_issue_comment | Discussion without edit |
| Submit review | github_create_pull_request_review | Approve/request changes |

## Remember

- Only specified fields are updated
- Closing a PR does NOT delete the branch
- Changing base branch may require conflict resolution
- Body supports Markdown formatting
- State: "open" or "closed" only
