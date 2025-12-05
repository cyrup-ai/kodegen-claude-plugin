---
name: github-fork-repository
description: >
  Fork an existing GitHub repository to your account or organization.
  WHEN: User wants to fork a repo, contribute to open source, copy someone else's project, says "fork this repo", "make a copy of this repository".
  WHEN NOT: Creating a new empty repo (use github_create_repository), cloning locally (use git_clone).
version: 0.1.0
---

# GitHub Fork Repository

## Core Concept

`mcp__plugin_kg_kodegen__github_fork_repository` creates a fork of an existing repository under your account or a specified organization. This is the standard way to contribute to open source projects.

## Key Parameters

### Required
- **owner** (string): Repository owner to fork from (user or org)
- **repo** (string): Repository name to fork

### Optional
- **organization** (string): Organization to fork to (defaults to authenticated user)

## Usage Examples

### Fork a repo to your account
```json
{
  "owner": "rust-lang",
  "repo": "rust"
}
```

### Fork a repo to an organization
```json
{
  "owner": "facebook",
  "repo": "react",
  "organization": "my-company"
}
```

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Fork existing repo | github_fork_repository | Creates fork on GitHub |
| Create new repo | github_create_repository | Creates empty repo |
| Clone to local | git_clone | Downloads repo locally |

## Remember

- Forking creates a copy linked to the original (upstream)
- You can then clone your fork locally to make changes
- Use pull requests to contribute changes back to the original
- Fork names default to the original repo name
- Forks maintain the link to upstream for easy syncing
