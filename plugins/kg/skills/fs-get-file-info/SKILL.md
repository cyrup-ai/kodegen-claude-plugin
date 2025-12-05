---
name: fs-get-file-info
description: >
  Get detailed metadata about a file or directory including size, timestamps, permissions, and line count.
  WHEN: User asks "how big is this file", "when was it modified", "file size", "file info", "check if exists", "file permissions", "line count".
  WHEN NOT: Reading file contents (use fs_read_file), listing directory contents (use fs_list_directory).
version: 0.1.0
---

# fs_get_file_info Guide

## Core Concept

`mcp__plugin_kg_kodegen__fs_get_file_info` retrieves detailed metadata about a file or directory without reading its contents. Returns size, creation time, modification time, permissions, type (file/directory), and line count for text files.

## Key Parameters

| Parameter | Required | Default | Description |
|-----------|----------|---------|-------------|
| `path` | Yes | - | Path to the file or directory |

## Usage Examples

### Get File Information
```json
{
  "path": "/project/src/main.rs"
}
```

**Returns**:
- Size in bytes
- Creation timestamp
- Last modified timestamp
- Permissions (read/write/execute)
- Type: "file" or "directory"
- Line count (for text files)

### Check Directory Metadata
```json
{
  "path": "/project/src"
}
```

### Verify File Exists
```json
{
  "path": "/project/config.json"
}
```
If file doesn't exist, returns an error - useful for existence checks.

## Practical Use Cases

### Before Large Operations
```json
// Check file size before reading
{
  "path": "/project/data/large-dataset.json"
}
// If size > 10MB, use fs_read_file with offset/length
```

### Check Modification Time
```json
// Was the file recently modified?
{
  "path": "/project/package-lock.json"
}
```

### Verify Permissions
```json
// Can I write to this file?
{
  "path": "/etc/hosts"
}
```

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Get file metadata | fs_get_file_info | Size, timestamps, permissions |
| Read file contents | fs_read_file | Get actual text/data |
| Check if file exists | fs_get_file_info | Returns error if not found |
| List directory contents | fs_list_directory | See files in a folder |
| Find files by pattern | fs_search | Pattern-based discovery |

## Remember

- **Metadata only**: Does not read file contents
- **Line count for text files**: Useful for estimating reading time
- **Existence check**: Error response indicates file doesn't exist
- **Works on directories**: Returns directory-specific metadata
- **Check before large reads**: Use size to decide read strategy
