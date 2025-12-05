---
name: github-create-repository
description: >
  Create a new GitHub repository with customizable settings.
  WHEN: User wants to create a new repo, says "make me a repository", "create repo", "new GitHub project", needs to initialize a new codebase on GitHub.
  WHEN NOT: Forking an existing repo (use github_fork_repository), cloning a repo locally (use git_clone).
version: 0.1.0
---

# GitHub Create Repository

## Core Concept

`mcp__plugin_kg_kodegen__github_create_repository` creates a new repository on GitHub under the authenticated user's account. It supports configuration of visibility, initialization options, merge strategies, and repository features.

## Key Parameters

### Required
- **name** (string): Repository name

### Optional
- **description** (string): Repository description
- **private** (boolean): Make repository private (default: false)
- **auto_init** (boolean): Initialize with README (default: false)
- **gitignore_template** (string): .gitignore template name (e.g., "Rust", "Node", "Python")
- **license_template** (string): License template (e.g., "mit", "apache-2.0", "gpl-3.0")
- **allow_squash_merge** (boolean): Allow squash merging (default: true)
- **allow_merge_commit** (boolean): Allow merge commits (default: true)
- **allow_rebase_merge** (boolean): Allow rebase merging (default: true)
- **delete_branch_on_merge** (boolean): Auto-delete head branches after merge
- **has_issues** (boolean): Enable issues (default: true)
- **has_projects** (boolean): Enable projects (default: true)
- **has_wiki** (boolean): Enable wiki (default: true)

## Usage Examples

### Create a minimal public repository
```json
{
  "name": "my-new-project"
}
```

### Create a private Rust project with full initialization
```json
{
  "name": "rust-cli-tool",
  "description": "A blazing-fast CLI tool written in Rust",
  "private": true,
  "auto_init": true,
  "gitignore_template": "Rust",
  "license_template": "mit",
  "delete_branch_on_merge": true
}
```

### Create a repo with specific merge settings
```json
{
  "name": "team-project",
  "description": "Team collaboration repository",
  "allow_squash_merge": true,
  "allow_merge_commit": false,
  "allow_rebase_merge": false,
  "has_wiki": false
}
```

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Create new empty repo | github_create_repository | Creates on GitHub |
| Fork existing repo | github_fork_repository | Copies existing repo |
| Clone repo locally | git_clone | Downloads to local machine |
| Initialize local repo | terminal (git init) | Local-only operation |

## Remember

- Requires GITHUB_TOKEN environment variable for authentication
- Repository name must be unique within your account
- Use `gitignore_template` and `license_template` for common templates
- Private repos require a GitHub plan that supports them
- Returns clone_url and html_url for immediate use
