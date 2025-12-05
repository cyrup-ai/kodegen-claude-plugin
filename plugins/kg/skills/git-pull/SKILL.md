---
name: git-pull
description: >
  Fetch from remote and merge into current branch.
  WHEN: User wants to update from remote, sync with upstream, "git pull", get latest changes.
  WHEN NOT: Only want to download without merge (use git_fetch), pushing changes (use git_push).
version: 0.1.0
---

# git_pull - Fetch and Merge

## Core Concept

`mcp__plugin_kg_kodegen__git_pull` combines fetch and merge in one operation. Downloads changes from remote and integrates them into your current branch.

## Key Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| path | string | Yes | - | Path to repository |
| remote | string | No | "origin" | Remote name |
| fast_forward | boolean | No | true | Allow fast-forward |
| auto_commit | boolean | No | true | Auto-commit merge |

## Usage Examples

### Standard pull
```json
{
  "path": "."
}
```

### Pull from specific remote
```json
{
  "path": ".",
  "remote": "upstream"
}
```

### Fast-forward only (fails if needs merge)
```json
{
  "path": ".",
  "fast_forward": true
}
```

### No fast-forward (always merge commit)
```json
{
  "path": ".",
  "fast_forward": false
}
```

## Pull vs Fetch

| Operation | Downloads | Merges | Safe to run? |
|-----------|-----------|--------|--------------|
| pull | Yes | Yes | Can cause conflicts |
| fetch | Yes | No | Always safe |

## Merge Outcomes

- `fast_forward`: Clean linear update
- `merge_commit`: Created merge commit
- `already_up_to_date`: Nothing new

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Update branch | git_pull | Fetch + merge |
| Download only | git_fetch | Preview first |
| Push changes | git_push | Upload to remote |
| See what changed | git_diff | After fetch, before pull |

## Remember

- Uncommitted changes may cause conflicts - commit or stash first
- Use git_fetch first for more control
- Merge conflicts require manual resolution
- Fast-forward only prevents unexpected merges
- Always pull before starting new work
