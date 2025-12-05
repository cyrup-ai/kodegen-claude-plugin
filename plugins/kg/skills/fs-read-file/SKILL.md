---
name: fs-read-file
description: >
  Read file contents with optional line offset and length. Supports text, images (base64), and URLs.
  WHEN: User wants to see file contents, read code, view text, show me the file, cat file, read from URL.
  WHEN NOT: Reading multiple files (use fs_read_multiple_files for efficiency), searching file contents (use fs_search).
version: 0.1.0
---

# fs_read_file Guide

## Core Concept

`mcp__plugin_kg_kodegen__fs_read_file` reads the contents of a file, URL, or image. For text files, it supports line-based offset and length parameters for reading specific portions. Images are returned as base64. URLs are fetched and returned as text.

## Key Parameters

| Parameter | Required | Default | Description |
|-----------|----------|---------|-------------|
| `path` | Yes | - | File path or URL to read |
| `offset` | No | 0 | Line offset (positive: start line, negative: tail) |
| `length` | No | null | Maximum lines to read (null = default limit) |
| `is_url` | No | false | Auto-detected, but can force URL mode |

## Usage Examples

### Read Entire File
```json
{
  "path": "/project/src/main.rs"
}
```

### Read Specific Lines (0-indexed)
```json
{
  "path": "/project/src/main.rs",
  "offset": 10,
  "length": 20
}
```
Reads lines 10-29 (20 lines starting from line 10).

### Read Last N Lines (Tail)
```json
{
  "path": "/project/logs/app.log",
  "offset": -50
}
```
Reads the last 50 lines (like `tail -n 50`).

### Read from URL
```json
{
  "path": "https://raw.githubusercontent.com/user/repo/main/README.md"
}
```

### Read Image (Returns Base64)
```json
{
  "path": "/project/assets/logo.png"
}
```

## Offset Behavior

| Offset Value | Behavior |
|--------------|----------|
| `0` (default) | Start from beginning |
| `10` | Start from line 10 (skip first 10 lines) |
| `-50` | Read last 50 lines (tail mode) |
| `-100` | Read last 100 lines |

## Common Patterns

### Read Config File
```json
{
  "path": "/project/package.json"
}
```

### Read Function Definition
```json
// First search for the function, then read around it
{
  "path": "/project/src/utils.ts",
  "offset": 45,
  "length": 30
}
```

### Check Log Tail
```json
{
  "path": "/var/log/application.log",
  "offset": -100
}
```

### Read Large File Efficiently
```json
// First, check size with fs_get_file_info
// Then read in chunks
{
  "path": "/project/data/large.csv",
  "offset": 0,
  "length": 100
}
```

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Read single file | fs_read_file | Direct file access |
| Read multiple files | fs_read_multiple_files | Parallel, more efficient |
| Search then read | fs_search + fs_read_file | Find location first |
| Read file metadata | fs_get_file_info | Size, timestamps (no content) |
| List directory | fs_list_directory | See available files |

## Remember

- **Line-based offset/length**: Works in lines, not bytes
- **Negative offset = tail**: `-50` means last 50 lines
- **Images return base64**: Binary files are encoded
- **URLs auto-detected**: Or force with `is_url: true`
- **Use fs_read_multiple_files for batch**: More efficient for 2+ files
- **Check size first**: Use fs_get_file_info for large files
