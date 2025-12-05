---
name: git-add
description: >
  Stage file changes for commit in a Git repository.
  WHEN: User wants to stage changes, "git add", prepare files for commit, stage specific files, stage all changes.
  WHEN NOT: Just viewing changes (use git_diff), checking repository status (use git_status).
version: 0.1.0
---

# git_add - Stage Files for Commit

## Core Concept

`mcp__plugin_kg_kodegen__git_add` stages file changes to the Git index (staging area). The index holds all staged changes that will be included in the next commit. This is the intermediate step between making changes and committing them.

## Key Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| path | string | Yes | - | Path to repository root |
| paths | string[] | No | [] | Specific file paths to stage |
| all | boolean | No | false | Stage all modified files |
| force | boolean | No | false | Force add files matching .gitignore |

## Usage Examples

### Stage specific files
```json
{
  "path": "/repo",
  "paths": ["src/main.rs", "Cargo.toml"]
}
```

### Stage all modified files
```json
{
  "path": "/repo",
  "all": true
}
```

### Stage a single file
```json
{
  "path": "/repo",
  "paths": ["README.md"]
}
```

### Force-add ignored files
```json
{
  "path": "/repo",
  "paths": ["secret.key"],
  "force": true
}
```

## Common Workflows

1. **Selective staging**: Stage specific files for focused commits
2. **Bulk staging**: Use `all: true` to stage everything at once
3. **Pre-commit review**: Stage files, then use git_status to verify

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Stage files | git_add | Prepares files for commit |
| View changes | git_diff | Shows what changed without staging |
| Check status | git_status | Shows staged vs unstaged files |
| Commit changes | git_commit | Creates commit from staged files |

## Remember

- This tool ONLY modifies the Git index, never working directory files
- Safe to call multiple times on the same files (idempotent)
- Use `all: true` OR provide `paths`, not both
- Files can be modified after staging - re-stage if needed
- Use git_status after staging to verify what will be committed
