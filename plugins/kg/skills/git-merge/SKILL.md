---
name: git-merge
description: >
  Merge a branch or commit into the current branch.
  WHEN: User wants to merge branches, integrate feature branch, combine changes, "git merge".
  WHEN NOT: Want fetch+merge together (use git_pull), just fetching (use git_fetch).
version: 0.1.0
---

# git_merge - Merge Branches

## Core Concept

`mcp__plugin_kg_kodegen__git_merge` integrates changes from one branch into the current branch. Supports fast-forward and merge commit strategies.

## Key Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| path | string | Yes | - | Path to repository |
| branch | string | Yes | - | Branch/commit to merge |
| fast_forward | boolean | No | true | Allow fast-forward merges |
| auto_commit | boolean | No | true | Auto-create merge commit |

## Usage Examples

### Standard merge (fast-forward if possible)
```json
{
  "path": ".",
  "branch": "feature"
}
```

### Force merge commit (no fast-forward)
```json
{
  "path": ".",
  "branch": "feature",
  "fast_forward": false
}
```

### Merge without auto-commit
```json
{
  "path": ".",
  "branch": "develop",
  "auto_commit": false
}
```

## Merge Strategies

| Strategy | When used | Result |
|----------|-----------|--------|
| Fast-forward | Linear history | Moves HEAD pointer |
| Merge commit | Diverged history | Creates merge commit |

## Merge Outcomes

- `fast_forward`: HEAD moved directly to target
- `merge_commit`: New merge commit created
- `already_up_to_date`: No changes to merge

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Merge branch | git_merge | Integrates into current |
| Fetch + merge | git_pull | Combined operation |
| Preview changes | git_diff | See what will be merged |
| Switch branch | git_checkout | Change context first |

## WARNING

- Merge can fail with conflicts (requires manual resolution)
- `auto_commit: false` leaves changes staged for review
- Always ensure clean working directory before merge

## Remember

- Merges INTO current branch FROM specified branch
- Fast-forward requires linear history
- Conflicts cause merge to fail
- Use git_diff first to preview changes
- Creates new commit when not fast-forward
