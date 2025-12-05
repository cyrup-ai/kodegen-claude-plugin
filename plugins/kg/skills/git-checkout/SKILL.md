---
name: git-checkout
description: >
  Switch branches, checkout commits, or restore files from a Git reference.
  WHEN: User wants to switch branch, checkout a commit, restore file to previous state, create and switch to new branch.
  WHEN NOT: Creating a branch without switching (use git_branch_create), viewing branch list (use git_branch_list).
version: 0.1.0
---

# git_checkout - Switch Branches or Restore Files

## Core Concept

`mcp__plugin_kg_kodegen__git_checkout` switches between branches, checks out specific commits/tags, or restores files from a reference. Without paths, it switches the entire working directory. With paths, it restores only those files.

## Key Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| path | string | Yes | - | Path to repository |
| target | string | Yes | - | Branch, tag, or commit hash |
| paths | string[] | No | null | Files to restore (without: switches branch) |
| create | boolean | No | false | Create branch before checkout |
| force | boolean | No | false | Discard local changes |

## Usage Examples

### Switch to existing branch
```json
{
  "path": ".",
  "target": "main"
}
```

### Create and checkout new branch
```json
{
  "path": ".",
  "target": "feature-x",
  "create": true
}
```

### Checkout specific commit
```json
{
  "path": ".",
  "target": "a1b2c3d"
}
```

### Checkout tag
```json
{
  "path": ".",
  "target": "v1.0.0"
}
```

### Restore specific files from another branch
```json
{
  "path": ".",
  "target": "main",
  "paths": ["config.json", "src/app.rs"]
}
```

### Force checkout (discard changes)
```json
{
  "path": ".",
  "target": "develop",
  "force": true
}
```

## Reference Detection

The tool automatically detects reference type:
- **Commits**: 7-40 hex characters (e.g., `a1b2c3d`)
- **Tags**: Start with `v` followed by digit (e.g., `v1.0.0`)
- **Branches**: Everything else (e.g., `main`, `feature/new-ui`)

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Switch branch | git_checkout | Changes working directory |
| Create branch only | git_branch_create | Creates without switching |
| Restore files | git_checkout + paths | Cherry-pick files from reference |
| View branches | git_branch_list | Lists available branches |

## WARNING

- **force=true** discards ALL uncommitted changes permanently
- Checkout fails if local changes would be overwritten (unless force=true)
- `create` flag only works with branch names, not commits or tags

## Remember

- Without `paths`: switches entire branch/commit
- With `paths`: restores only specified files, keeps current branch
- Use `create: true` for new branches in one step
- Short commit SHAs must be 7+ characters to avoid ambiguity
- File restoration does not change current branch
