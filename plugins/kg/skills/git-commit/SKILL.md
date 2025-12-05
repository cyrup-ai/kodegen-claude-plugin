---
name: git-commit
description: >
  Create a commit with staged changes in a Git repository.
  WHEN: User wants to commit changes, save work, create a commit, "git commit".
  WHEN NOT: Need to stage files first (use git_add), pushing to remote (use git_push).
version: 0.1.0
---

# git_commit - Create Commits

## Core Concept

`mcp__plugin_kg_kodegen__git_commit` creates a new commit with the currently staged changes. Can optionally auto-stage all modified files and specify custom author information.

## Key Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| path | string | Yes | - | Path to repository |
| message | string | Yes | - | Commit message |
| all | boolean | No | false | Auto-stage all modified files |
| author_name | string | No | git config | Override author name |
| author_email | string | No | git config | Override author email |

## Usage Examples

### Simple commit (staged files)
```json
{
  "path": ".",
  "message": "Fix: update dependencies"
}
```

### Commit all modified files
```json
{
  "path": ".",
  "message": "Refactor: simplify error handling",
  "all": true
}
```

### Commit with custom author
```json
{
  "path": ".",
  "message": "Docs: update README",
  "author_name": "Alice Dev",
  "author_email": "alice@example.com"
}
```

### Full example with all options
```json
{
  "path": ".",
  "message": "Release: v1.0.0",
  "all": true,
  "author_name": "Release Bot",
  "author_email": "release@example.com"
}
```

## Commit Message Best Practices

Use conventional commit prefixes:
- `Fix:` - Bug fixes
- `Feat:` - New features
- `Refactor:` - Code restructuring
- `Docs:` - Documentation changes
- `Test:` - Test additions/changes
- `Chore:` - Maintenance tasks

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Stage files | git_add | Prepare files for commit |
| Create commit | git_commit | Save staged changes |
| View staged | git_status | See what will be committed |
| Push commits | git_push | Share with remote |

## Remember

- Set `all: true` to auto-stage modified files before commit
- Custom author requires BOTH name and email together
- Returns commit SHA (7-char short form) and file count
- Each call creates a NEW commit (not idempotent)
- Stage specific files with git_add for selective commits
