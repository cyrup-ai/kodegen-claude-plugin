---
name: github-get-pull-request-status
description: >
  Get detailed status of a pull request including merge status, CI checks, and review state.
  WHEN: User asks "can this be merged", "what's the PR status", "are checks passing", "is PR ready".
  WHEN NOT: Getting file list (use github_get_pull_request_files), getting reviews (use github_get_pull_request_reviews).
version: 0.1.0
---

# GitHub Get Pull Request Status

## Core Concept

`mcp__plugin_kg_kodegen__github_get_pull_request_status` provides comprehensive status information about a PR including whether it can be merged, CI check results, and overall health.

## Key Parameters

### Required (all)
- **owner**: Repository owner
- **repo**: Repository name
- **pr_number**: Pull request number

## Usage Examples

### Check PR Status
```json
{
  "owner": "octocat",
  "repo": "hello-world",
  "pr_number": 42
}
```

## Response Fields

Returns `GitHubGetPrStatusOutput` with:
- **state**: "open" or "closed"
- **mergeable**: true/false/null (null = still calculating)
- **checks_status**: "pass", "fail", or "pending"
- **checks_count**: Total number of checks
- **checks_passed**: Number of passing checks
- **checks_failed**: Number of failed checks

## Mergeable States

The underlying `mergeable_state` indicates:
- **clean**: Ready to merge, no conflicts
- **dirty**: Has merge conflicts
- **blocked**: Blocked by branch protection
- **unstable**: Checks failing but mergeable
- **behind**: Base branch has new commits
- **draft**: PR is still a draft

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Check merge readiness | github_get_pull_request_status | Mergeable + checks |
| Get changed files | github_get_pull_request_files | File list |
| Get reviews | github_get_pull_request_reviews | Approval status |
| Merge the PR | github_merge_pull_request | After status is good |

## Remember

- Check this BEFORE attempting to merge
- mergeable=null means GitHub is still calculating
- Use retry logic if mergeable is null
- checks_status summarizes all CI results
- Combine with reviews check for full readiness
