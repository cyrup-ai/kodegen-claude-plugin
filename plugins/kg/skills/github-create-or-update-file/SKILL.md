---
name: github-create-or-update-file
description: >
  Create or update a single file in a GitHub repository via API.
  WHEN: User wants to edit a file directly on GitHub, quick single-file update without cloning, says "update this file on GitHub", "create file in repo".
  WHEN NOT: Multiple files at once (use github_push_files), local file editing (use fs_write_file), complex changes (clone and push).
version: 0.1.0
---

# GitHub Create or Update File

## Core Concept

`mcp__plugin_kg_kodegen__github_create_or_update_file` creates or updates a single file in a GitHub repository directly via the API. This is ideal for quick edits without needing a local clone.

## Key Parameters

### Required
- **owner** (string): Repository owner
- **repo** (string): Repository name
- **path** (string): File path in the repository
- **message** (string): Commit message
- **content** (string): File content (plain text, auto-encoded to base64)

### Optional
- **branch** (string): Target branch (defaults to default branch)
- **sha** (string): Current file SHA (REQUIRED for updates, omit for new files)

## Usage Examples

### Create a new file
```json
{
  "owner": "myuser",
  "repo": "my-project",
  "path": "docs/CONTRIBUTING.md",
  "message": "Add contributing guidelines",
  "content": "# Contributing\n\nWelcome! Please read these guidelines..."
}
```

### Update an existing file (requires SHA)
```json
{
  "owner": "myuser",
  "repo": "my-project",
  "path": "README.md",
  "message": "Update README with new features",
  "content": "# My Project\n\nUpdated content here...",
  "sha": "abc123def456..."
}
```

### Create file on a specific branch
```json
{
  "owner": "myuser",
  "repo": "my-project",
  "path": "config/settings.json",
  "message": "Add production settings",
  "content": "{\"env\": \"production\"}",
  "branch": "feature/config"
}
```

## Getting the SHA for Updates

To update an existing file, you MUST provide the current SHA:
1. Use `github_get_file_contents` to get the file
2. Extract the `sha` field from the response
3. Include it in your update request

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Single file create/update | github_create_or_update_file | Quick API edit |
| Multiple files at once | github_push_files | Batch operation |
| Local file editing | fs_write_file + git push | Full local workflow |
| Read before update | github_get_file_contents | Get current SHA |

## Remember

- For UPDATES, you MUST provide the current file SHA
- For NEW files, omit the SHA parameter
- Content is plain text (automatically base64 encoded)
- Creates a commit for each operation
- Cannot update multiple files atomically (use github_push_files)
