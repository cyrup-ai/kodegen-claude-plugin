---
name: fs-list-directory
description: >
  List files and directories in a specified path with type indicators.
  WHEN: User asks "what's in this folder", "list files", "show directory", "ls", "dir", "folder contents".
  WHEN NOT: Finding files by pattern (use fs_search with search_in: "filenames"), reading file contents (use fs_read_file).
version: 0.1.0
---

# fs_list_directory Guide

## Core Concept

`mcp__plugin_kg_kodegen__fs_list_directory` lists all files and directories in a specified path. Returns entries prefixed with `[DIR]` or `[FILE]` to distinguish types. This is a simple, one-level listing - it does not recurse into subdirectories.

## Key Parameters

| Parameter | Required | Default | Description |
|-----------|----------|---------|-------------|
| `path` | Yes | - | Path to the directory to list |
| `include_hidden` | No | false | Include hidden files (starting with `.`) |

## Usage Examples

### List Directory Contents
```json
{
  "path": "/project/src"
}
```

**Returns**:
```
[DIR] components
[DIR] utils
[FILE] main.ts
[FILE] index.ts
```

### Include Hidden Files
```json
{
  "path": "/project",
  "include_hidden": true
}
```

**Returns**:
```
[DIR] .git
[DIR] src
[FILE] .gitignore
[FILE] .env
[FILE] package.json
```

### List Home Directory
```json
{
  "path": "~"
}
```

## Common Use Cases

### Explore Project Structure
```json
{
  "path": "/project"
}
```

### Check for Config Files
```json
{
  "path": "/project",
  "include_hidden": true
}
// Look for .env, .gitignore, .eslintrc, etc.
```

### Verify File Placement
```json
{
  "path": "/project/src/components"
}
// Check if new component was created in correct location
```

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Simple directory listing | fs_list_directory | One-level, type-annotated list |
| Find files by pattern | fs_search (filenames) | `search_in: "filenames"` with pattern |
| Find files by content | fs_search (content) | Search inside files |
| Read specific file | fs_read_file | When you know the file path |
| Get file details | fs_get_file_info | Size, timestamps, permissions |

## fs_list_directory vs fs_search

| Feature | fs_list_directory | fs_search |
|---------|-------------------|-----------|
| Speed | Instant | Fast (but searches more) |
| Scope | Single directory | Recursive by default |
| Pattern matching | No | Yes (regex/glob) |
| Hidden files | Optional flag | Configurable |
| Best for | Quick look at folder | Finding specific files |

## Remember

- **One level only**: Does not recurse into subdirectories
- **Type prefixes**: `[DIR]` for directories, `[FILE]` for files
- **Hidden files require flag**: Use `include_hidden: true` for dotfiles
- **Use fs_search for patterns**: This tool doesn't support wildcards
- **Quick exploration**: Best for "what's in this folder?" questions
