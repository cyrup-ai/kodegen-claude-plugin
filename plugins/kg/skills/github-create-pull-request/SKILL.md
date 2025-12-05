---
name: github-create-pull-request
description: >
  Create a new pull request in a GitHub repository from a head branch to a base branch.
  WHEN: User wants to open PR, create merge request, submit changes for review, propose changes.
  WHEN NOT: Creating issue (use github_create_issue), updating existing PR (use github_update_pull_request).
version: 0.1.0
---

# GitHub Create Pull Request

## Core Concept

`mcp__plugin_kg_kodegen__github_create_pull_request` creates a new pull request from a source branch (head) to a target branch (base). Supports draft PRs and cross-fork PRs.

## Key Parameters

### Required
- **owner**: Repository owner
- **repo**: Repository name
- **title**: PR title
- **head**: Source branch name (e.g., "feature-branch")
- **base**: Target branch name (e.g., "main")

### Optional
- **body**: PR description (supports Markdown)
- **draft**: true to create as draft PR (default: false)
- **maintainer_can_modify**: Allow maintainers to push to head branch (default: true)

## Usage Examples

### Basic Pull Request
```json
{
  "owner": "octocat",
  "repo": "hello-world",
  "title": "Add new feature",
  "body": "This PR adds a new feature that improves...",
  "head": "feature-branch",
  "base": "main"
}
```

### Draft Pull Request
```json
{
  "owner": "octocat",
  "repo": "hello-world",
  "title": "WIP: Experimental feature",
  "head": "experimental",
  "base": "develop",
  "draft": true
}
```

### Cross-Fork PR (from your fork to upstream)
```json
{
  "owner": "upstream-org",
  "repo": "project",
  "title": "Fix bug in authentication",
  "head": "your-username:fix-auth",
  "base": "main"
}
```

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Create PR | github_create_pull_request | New pull request |
| Create issue | github_create_issue | Bug reports, features |
| Update PR | github_update_pull_request | Change title/body/state |
| Merge PR | github_merge_pull_request | Complete the PR |

## Remember

- Head branch must have commits ahead of base
- Draft PRs cannot be merged until marked ready
- For cross-fork PRs, use "username:branch" format for head
- Body supports Markdown with @mentions and #references
- Each call creates a NEW PR (not idempotent)
