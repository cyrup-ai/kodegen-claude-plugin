---
name: git-stash
description: >
  Save or restore uncommitted changes temporarily without committing.
  WHEN: User says "stash changes", "save work temporarily", "stash my changes", "pop stash", "restore stash".
  WHEN NOT: Want to commit changes (use git_commit), want to discard changes (use git_reset).
version: 0.1.0
---

# Git Stash

## Core Concept

`mcp__plugin_kg_kodegen__git_stash` temporarily saves uncommitted changes without committing. Supports two operations: "save" to stash changes, and "pop" to restore and remove the most recent stash.

## Key Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| path | string | Yes | - | Path to the Git repository |
| operation | string | No | "save" | Operation: "save" or "pop" |
| message | string | No | - | Description for saved stash |
| include_untracked | boolean | No | true | Include new untracked files |

## Usage Examples

### Save changes with message
```json
{
  "path": ".",
  "operation": "save",
  "message": "WIP: user authentication feature",
  "include_untracked": true
}
```

### Save changes (minimal)
```json
{
  "path": ".",
  "operation": "save"
}
```

### Restore most recent stash
```json
{
  "path": ".",
  "operation": "pop"
}
```

### Save without untracked files
```json
{
  "path": "/project",
  "operation": "save",
  "message": "Quick save before switching",
  "include_untracked": false
}
```

## Common Workflows

### Context Switching
1. Save current work: `git_stash` with operation="save"
2. Switch branch: `git_checkout`
3. Do other work
4. Return to original branch: `git_checkout`
5. Restore work: `git_stash` with operation="pop"

### Clean Working Directory
1. Save all changes: `git_stash` with include_untracked=true
2. Perform operations requiring clean state
3. Restore: `git_stash` with operation="pop"

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Save temporarily | git_stash (save) | Preserves work without commit |
| Restore from stash | git_stash (pop) | Applies and removes stash |
| Commit changes | git_commit | Permanent history |
| Discard changes | git_reset | Removes modifications |

## Remember

- Stash is LIFO (Last-In-First-Out) stack
- Pop applies most recent stash and removes it
- Use message parameter to identify stashes later
- include_untracked=true captures new files (recommended)
- Stashes are local only, not pushed to remotes
- Pop fails gracefully if stash is empty
