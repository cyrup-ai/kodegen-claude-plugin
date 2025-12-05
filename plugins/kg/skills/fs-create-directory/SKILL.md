---
name: fs-create-directory
description: >
  Create directories including nested paths in a single operation.
  WHEN: User wants to create a folder, set up directory structure, make a new directory, organize project layout, scaffold folders.
  WHEN NOT: Creating a file with content (use fs_write_file instead).
version: 0.1.0
---

# fs_create_directory Guide

## Core Concept

`mcp__plugin_kg_kodegen__fs_create_directory` creates a new directory or ensures a directory exists. It automatically creates all parent directories in the path (like `mkdir -p`), making it ideal for setting up project structures in one operation.

## Key Parameters

| Parameter | Required | Default | Description |
|-----------|----------|---------|-------------|
| `path` | Yes | - | Absolute or relative path to the directory to create |

## Usage Examples

### Create a Simple Directory
```json
{
  "path": "/project/src/components"
}
```

### Create Nested Directory Structure
```json
{
  "path": "/project/src/features/auth/hooks"
}
```
Creates `/project/src/features/auth/hooks` and all parent directories.

### Project Scaffolding
```json
// Run multiple times for complete structure
{"path": "/myapp/src/components"}
{"path": "/myapp/src/utils"}
{"path": "/myapp/src/hooks"}
{"path": "/myapp/tests"}
{"path": "/myapp/docs"}
```

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Create folder(s) | fs_create_directory | Creates directories with parent paths |
| Create file with content | fs_write_file | Creates file and writes content |
| Check if path exists | fs_get_file_info | Returns metadata including type |

## Remember

- **Creates parent directories automatically**: No need to create each level separately
- **Idempotent**: Safe to run multiple times - won't error if directory exists
- **Only creates directories**: Use fs_write_file to create files
- **Use absolute paths**: Agent threads reset cwd between calls
- **Validates paths**: Automatically checks against allowed directories
