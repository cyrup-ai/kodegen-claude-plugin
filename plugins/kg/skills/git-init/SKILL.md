---
name: git-init
description: >
  Initialize a new Git repository at a specified path.
  WHEN: User wants to start version control, create new repo, "git init", initialize git tracking.
  WHEN NOT: Repository already exists (use git_open), cloning from remote (use git_clone).
version: 0.1.0
---

# git_init - Initialize Repository

## Core Concept

`mcp__plugin_kg_kodegen__git_init` creates a new Git repository at the specified path. Supports both normal repositories (with working directory) and bare repositories (for servers).

## Key Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| path | string | Yes | - | Where to initialize |
| bare | boolean | No | false | Create bare repository |
| initial_branch | string | No | default | Initial branch name (informational) |

## Usage Examples

### Normal repository
```json
{
  "path": "/home/user/my-project"
}
```

### Bare repository (server-style)
```json
{
  "path": "/var/git/my-repo.git",
  "bare": true
}
```

### Initialize in current directory
```json
{
  "path": "."
}
```

## Normal vs Bare Repositories

| Type | Has working dir? | Use case |
|------|------------------|----------|
| Normal | Yes | Local development |
| Bare | No | Centralized/server repos |

## Common Workflows

1. **Local project setup**:
   - git_init -> create files -> git_add -> git_commit

2. **Server repository**:
   - git_init (bare) -> clients git_clone from it

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Create new repo | git_init | Starts fresh |
| Clone existing | git_clone | Downloads from remote |
| Open existing | git_open | Already initialized |

## Remember

- Target path must NOT exist before calling
- After init, repository is empty (no commits)
- Bare repos: typically use .git extension by convention
- Normal repos have .git subdirectory
- First commit requires git_add then git_commit
