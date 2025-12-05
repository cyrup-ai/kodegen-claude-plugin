---
name: github-get-file-contents
description: >
  Get file or directory contents directly from GitHub without cloning.
  WHEN: User wants to read a file from GitHub, view remote file contents, says "show me file X from repo Y", "what's in this GitHub file", needs file without cloning.
  WHEN NOT: Reading local files (use fs_read_file), need to edit files (clone first or use github_create_or_update_file).
version: 0.1.0
---

# GitHub Get File Contents

## Core Concept

`mcp__plugin_kg_kodegen__github_get_file_contents` retrieves file or directory contents directly from GitHub via the API. No local clone required. Can get file contents or list directory entries.

## Key Parameters

### Required
- **owner** (string): Repository owner
- **repo** (string): Repository name
- **path** (string): File or directory path

### Optional
- **ref_name** (string): Branch, tag, or commit (defaults to default branch)

## Usage Examples

### Get a file from default branch
```json
{
  "owner": "rust-lang",
  "repo": "rust",
  "path": "README.md"
}
```

### Get a file from specific branch
```json
{
  "owner": "facebook",
  "repo": "react",
  "path": "packages/react/package.json",
  "ref_name": "main"
}
```

### List directory contents
```json
{
  "owner": "torvalds",
  "repo": "linux",
  "path": "kernel"
}
```

### Get file from a tag
```json
{
  "owner": "nodejs",
  "repo": "node",
  "path": "package.json",
  "ref_name": "v20.0.0"
}
```

## Output Fields

For files:
- **content**: Decoded file content
- **sha**: File blob SHA
- **size**: File size in bytes
- **html_url**: GitHub web URL

For directories:
- **directory_contents**: Array of entries with name, path, type, sha

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Read remote file | github_get_file_contents | No clone needed |
| Read local file | fs_read_file | Local filesystem |
| Update remote file | github_create_or_update_file | GitHub API |
| Clone then read | git_clone + fs_read_file | Full local copy |

## Remember

- Content is automatically base64-decoded
- Can read from any branch, tag, or commit SHA
- Works on public repos without auth, private repos need GITHUB_TOKEN
- Large files may be truncated (use git clone for large repos)
- Directory path returns list of entries, not contents
