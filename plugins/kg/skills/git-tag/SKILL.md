---
name: git-tag
description: >
  Create, list, or delete Git tags for marking release points.
  WHEN: User says "create tag", "list tags", "delete tag", "version tag", "release tag", "mark release".
  WHEN NOT: Creating branch (use git_branch_create), checking out tag (use git_checkout).
version: 0.1.0
---

# Git Tag

## Core Concept

`mcp__plugin_kg_kodegen__git_tag` manages repository tags for marking release points. Supports creating lightweight tags (simple pointers) and annotated tags (with metadata), listing all tags, and deleting tags.

## Key Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| path | string | Yes | - | Path to the Git repository |
| operation | string | No | "list" | Operation: "create", "delete", or "list" |
| name | string | create/delete | - | Tag name (required for create/delete) |
| message | string | No | - | Tag message (creates annotated tag) |
| target | string | No | HEAD | Commit to tag |
| force | boolean | No | false | Overwrite existing tag |

## Usage Examples

### List all tags
```json
{
  "path": ".",
  "operation": "list"
}
```

### Create lightweight tag
```json
{
  "path": ".",
  "operation": "create",
  "name": "v1.0.0"
}
```

### Create annotated tag with message
```json
{
  "path": ".",
  "operation": "create",
  "name": "v2.0.0",
  "message": "Release 2.0.0 - Major feature update"
}
```

### Tag specific commit
```json
{
  "path": ".",
  "operation": "create",
  "name": "v1.5.0",
  "target": "abc1234",
  "message": "Backport release"
}
```

### Delete a tag
```json
{
  "path": ".",
  "operation": "delete",
  "name": "v0.9.0-beta"
}
```

### Force update existing tag
```json
{
  "path": ".",
  "operation": "create",
  "name": "latest",
  "force": true
}
```

## Tag Types

| Type | Created By | Use Case |
|------|-----------|----------|
| Lightweight | No message | Quick markers, local use |
| Annotated | With message | Releases, shared tags |

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Mark release | git_tag (create) | Creates version marker |
| List versions | git_tag (list) | Shows all tags |
| Remove old tag | git_tag (delete) | Cleanup |
| Checkout tag | git_checkout | Switch to tagged commit |

## Best Practices

- Use semantic versioning: v1.2.3
- Use annotated tags for releases (include message)
- Don't force-update published tags
- List tags before creating to avoid duplicates
- Push tags separately: `git push --tags`

## Remember

- Annotated tags store tagger info and message
- Lightweight tags are just commit pointers
- force=true overwrites existing tags (use carefully)
- Tags are local until pushed to remote
- Delete operation is permanent
