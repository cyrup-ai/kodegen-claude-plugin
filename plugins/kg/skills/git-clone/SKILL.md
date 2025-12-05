---
name: git-clone
description: >
  Clone a remote Git repository to a local path.
  WHEN: User wants to clone a repo, download a project, get a copy of remote repository.
  WHEN NOT: Repository already exists locally (use git_open), initializing new repo (use git_init).
version: 0.1.0
---

# git_clone - Clone Remote Repository

## Core Concept

`mcp__plugin_kg_kodegen__git_clone` downloads a complete copy of a remote Git repository to your local filesystem. Supports shallow cloning for faster downloads and branch-specific cloning.

## Key Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| url | string | Yes | - | Repository URL (https:// or git://) |
| path | string | Yes | - | Local destination path |
| branch | string | No | default | Specific branch to checkout |
| depth | number | No | full | Shallow clone depth (1 = latest only) |

## Usage Examples

### Basic clone (full history)
```json
{
  "url": "https://github.com/user/repo.git",
  "path": "./repo"
}
```

### Clone specific branch
```json
{
  "url": "https://github.com/user/repo.git",
  "path": "./repo",
  "branch": "develop"
}
```

### Shallow clone (faster)
```json
{
  "url": "https://github.com/user/repo.git",
  "path": "./repo",
  "depth": 1
}
```

### Shallow clone with branch
```json
{
  "url": "https://github.com/user/repo.git",
  "path": "./repo",
  "branch": "main",
  "depth": 1
}
```

## When to Use Shallow Clones

- Large repositories with long history
- CI/CD pipelines where full history not needed
- Quick initial downloads for iteration
- Bandwidth/disk space constraints

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Download remote repo | git_clone | Full copy to local |
| Open existing repo | git_open | Already have it locally |
| Create new repo | git_init | Starting fresh |
| Update existing | git_pull | Already cloned |

## Remember

- Destination path must NOT already exist
- Shallow clones limit some git operations (rebasing restrictions)
- URL must be accessible from current network
- Returns cloned branch name and metadata
- Makes network request (open_world operation)
