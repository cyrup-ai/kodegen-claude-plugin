---
name: github-get-commit
description: >
  Get detailed information about a specific commit.
  WHEN: User asks "what changed in commit X", "show me this commit", "what files were modified", needs commit details, diffs, or file changes.
  WHEN NOT: Listing commits (use github_list_commits), viewing commit history (use github_list_commits).
version: 0.1.0
---

# GitHub Get Commit

## Core Concept

`mcp__plugin_kg_kodegen__github_get_commit` retrieves detailed information about a specific commit including the commit message, author, stats, and file changes with diffs.

## Key Parameters

### Required
- **owner** (string): Repository owner
- **repo** (string): Repository name
- **commit_sha** (string): Full or abbreviated commit SHA

### Optional
- **page** (u32): Page number for file list
- **per_page** (u8): Files per page (max 100)

## Usage Examples

### Get commit details
```json
{
  "owner": "rust-lang",
  "repo": "rust",
  "commit_sha": "abc123def456789"
}
```

### Get commit with paginated files
```json
{
  "owner": "torvalds",
  "repo": "linux",
  "commit_sha": "def789abc123",
  "per_page": 50
}
```

## Output Fields

- **sha**: Full commit SHA
- **message**: Commit message
- **author_name/email**: Author info
- **committer_name/email**: Committer info
- **author_date/commit_date**: Timestamps
- **parents**: Parent commit SHAs
- **stats**: additions, deletions, total changes
- **files**: Array of changed files with:
  - filename, status (added/removed/modified/renamed)
  - additions, deletions, changes
  - patch (diff content)

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Specific commit details | github_get_commit | Full info + diffs |
| List recent commits | github_list_commits | Overview of history |
| Get file at commit | github_get_file_contents | Use commit SHA as ref_name |
| Compare commits | Use multiple github_get_commit | Manual comparison |

## Remember

- Returns full diff patches for each file
- Large commits may need pagination for files
- Use abbreviated SHA (7+ chars) or full SHA
- Parent SHAs useful for understanding merge commits
- Stats show overall additions/deletions count
