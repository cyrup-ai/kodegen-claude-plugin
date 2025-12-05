---
name: git-reset
description: >
  Reset repository HEAD to a specific state. DESTRUCTIVE with --hard mode.
  WHEN: User wants to undo commits, unstage changes, reset to specific state, "git reset".
  WHEN NOT: Want to keep changes (use git_stash), switching branches (use git_checkout).
version: 0.1.0
---

# git_reset - Reset Repository State

## Core Concept

`mcp__plugin_kg_kodegen__git_reset` moves HEAD to a specified commit with different levels of impact on index and working directory.

## Key Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| path | string | Yes | - | Path to repository |
| target | string | Yes | - | Target commit/ref |
| mode | string | No | "mixed" | soft, mixed, or hard |

## Reset Modes

| Mode | HEAD | Index | Working Dir | Use Case |
|------|------|-------|-------------|----------|
| soft | Moves | Unchanged | Unchanged | Undo commits, keep staged |
| mixed | Moves | Reset | Unchanged | Unstage, keep changes |
| hard | Moves | Reset | Reset | Complete reset (DESTRUCTIVE) |

## Usage Examples

### Soft reset (undo last commit, keep staged)
```json
{
  "path": ".",
  "target": "HEAD~1",
  "mode": "soft"
}
```

### Mixed reset (unstage all)
```json
{
  "path": ".",
  "target": "HEAD",
  "mode": "mixed"
}
```

### Hard reset (discard everything)
```json
{
  "path": ".",
  "target": "origin/main",
  "mode": "hard"
}
```

### Reset to specific commit
```json
{
  "path": ".",
  "target": "abc1234",
  "mode": "mixed"
}
```

## Target Specification

- Commit hash: `abc1234` or full SHA
- Relative: `HEAD~1`, `HEAD~5`, `HEAD^2`
- Branch: `main`, `origin/main`
- Tag: `v1.0.0`

## WARNING - Hard Reset

- **PERMANENTLY DESTROYS** uncommitted work
- Cannot be easily undone (check reflog)
- Never use on shared branches
- Back up important changes first

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Undo commit (keep changes) | git_reset soft | Preserves work |
| Unstage files | git_reset mixed | Keeps in working dir |
| Discard all changes | git_reset hard | Complete reset |
| Save changes for later | git_stash | Non-destructive |

## Remember

- Default mode is "mixed"
- Soft: recommit with different message
- Mixed: re-select what to stage
- Hard: last resort, destroys work
- Use reflog to recover from mistakes
- Safe to reset to same target multiple times
