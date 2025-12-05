---
name: fs-read-multiple-files
description: >
  Read multiple files in parallel for efficient batch reading.
  WHEN: User wants to read several files, compare files, batch reading, read these files, show me multiple files.
  WHEN NOT: Reading single file (use fs_read_file), searching across files (use fs_search).
version: 0.1.0
---

# fs_read_multiple_files Guide

## Core Concept

`mcp__plugin_kg_kodegen__fs_read_multiple_files` reads multiple files in parallel, returning results for all files including any individual errors. This is significantly more efficient than calling fs_read_file multiple times.

## Key Parameters

| Parameter | Required | Default | Description |
|-----------|----------|---------|-------------|
| `paths` | Yes | - | Array of file paths to read |
| `offset` | No | 0 | Line offset applied to all files |
| `length` | No | null | Max lines per file |

## Usage Examples

### Read Multiple Files
```json
{
  "paths": [
    "/project/src/index.ts",
    "/project/src/utils.ts",
    "/project/src/types.ts"
  ]
}
```

### Read with Offset/Length
```json
{
  "paths": [
    "/project/src/component-a.tsx",
    "/project/src/component-b.tsx"
  ],
  "offset": 0,
  "length": 50
}
```
Reads first 50 lines of each file.

### Read Configuration Files
```json
{
  "paths": [
    "/project/package.json",
    "/project/tsconfig.json",
    "/project/.eslintrc"
  ]
}
```

### Tail Multiple Log Files
```json
{
  "paths": [
    "/var/log/app/error.log",
    "/var/log/app/access.log"
  ],
  "offset": -100
}
```
Reads last 100 lines from each log file.

## Single String Shorthand

The `paths` parameter accepts both array and single string:

```json
// Array form
{"paths": ["file1.txt", "file2.txt"]}

// Single string (less common)
{"paths": "file.txt"}
```

## Error Handling

Individual file errors don't stop the entire operation:

```json
{
  "paths": [
    "/project/exists.txt",      // Success
    "/project/missing.txt",     // Error: not found
    "/project/also-exists.txt"  // Success
  ]
}
```
Returns results for successful reads and error messages for failures.

## Performance: Why Use This?

| Operation | fs_read_file x 5 | fs_read_multiple_files |
|-----------|------------------|------------------------|
| API Calls | 5 sequential | 1 parallel |
| Total Time | ~500ms | ~100ms |
| Efficiency | Low | High |

## Common Patterns

### Compare Similar Files
```json
{
  "paths": [
    "/project/src/Button.tsx",
    "/project/src/IconButton.tsx"
  ]
}
```

### Read Related Files Together
```json
{
  "paths": [
    "/project/src/api/users.ts",
    "/project/src/types/user.ts",
    "/project/src/hooks/useUser.ts"
  ]
}
```

### After Search Results
```json
// After fs_search finds files containing "TODO"
{
  "paths": [
    "/project/src/file1.ts",
    "/project/src/file2.ts",
    "/project/src/file3.ts"
  ]
}
```

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Read 2+ files | fs_read_multiple_files | Parallel, efficient |
| Read 1 file | fs_read_file | Simpler for single file |
| Find files first | fs_search | Get paths to read |
| Read large files | fs_read_file with offset | Control chunk size |

## Remember

- **Always use for 2+ files**: More efficient than multiple fs_read_file calls
- **Parallel execution**: All files read simultaneously
- **Partial success**: Returns results even if some files fail
- **Same offset/length for all**: Applied uniformly to all files
- **Combine with fs_search**: Search finds paths, this reads them
