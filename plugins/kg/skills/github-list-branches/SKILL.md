---
name: github-list-branches
description: >
  List all branches in a GitHub repository.
  WHEN: User asks "what branches exist", "show me the branches", "list remote branches", checking what branches are available on GitHub.
  WHEN NOT: Listing local branches (use git_branch_list), getting detailed branch info.
version: 0.1.0
---

# GitHub List Branches

## Core Concept

`mcp__plugin_kg_kodegen__github_list_branches` retrieves all branches from a GitHub repository via the API. Returns branch names, their HEAD SHA, and protection status.

## Key Parameters

### Required
- **owner** (string): Repository owner
- **repo** (string): Repository name

### Optional
- **page** (u32): Page number for pagination
- **per_page** (u8): Results per page (max 100)

## Usage Examples

### List all branches
```json
{
  "owner": "rust-lang",
  "repo": "rust"
}
```

### Paginated branch listing
```json
{
  "owner": "facebook",
  "repo": "react",
  "page": 2,
  "per_page": 50
}
```

## Output Fields

Each branch includes:
- **name**: Branch name
- **sha**: HEAD commit SHA
- **protected**: Whether branch is protected

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| List remote branches | github_list_branches | GitHub API |
| List local branches | git_branch_list | Local git operation |
| Get branch SHA | github_list_branches | Returns SHA for each branch |
| Create branch | github_create_branch | Uses SHA from this list |

## Remember

- Returns remote branches on GitHub
- Use the SHA field when creating new branches
- Protected branches cannot be deleted or force-pushed
- Pagination needed for repos with many branches
- Does NOT show local-only branches
