---
name: fs-delete-file
description: >
  Delete a single file permanently. DESTRUCTIVE operation.
  WHEN: User wants to remove a file, delete a specific file, clean up a file.
  WHEN NOT: Removing directories (use fs_delete_directory), moving files (use fs_move_file).
version: 0.1.0
---

# fs_delete_file Guide

## Core Concept

`mcp__plugin_kg_kodegen__fs_delete_file` permanently deletes a single file from the filesystem. This is a **destructive operation** that cannot be undone.

## WARNING

This operation is **PERMANENT** and **CANNOT BE UNDONE**. The file will be deleted, not moved to trash.

## Key Parameters

| Parameter | Required | Default | Description |
|-----------|----------|---------|-------------|
| `path` | Yes | - | Path to the file to delete |

## Usage Examples

### Delete a File
```json
{
  "path": "/project/src/old-component.tsx"
}
```

### Delete a Temporary File
```json
{
  "path": "/project/temp/cache.json"
}
```

### Delete a Log File
```json
{
  "path": "/var/log/myapp/debug.log"
}
```

## Error Cases

```json
// Fails if path is a directory
{
  "path": "/project/src"  // Error: use fs_delete_directory instead
}

// Fails if file doesn't exist
{
  "path": "/project/nonexistent.txt"  // Error: file not found
}
```

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Delete single file | fs_delete_file | File-only deletion |
| Delete directory | fs_delete_directory | Requires recursive confirmation |
| Move file elsewhere | fs_move_file | Non-destructive relocation |
| Rename file | fs_move_file | Same directory, different name |

## Remember

- **PERMANENT**: Cannot be undone - no trash/recycle bin
- **Files only**: Cannot delete directories (use fs_delete_directory)
- **Verify path**: Check the file exists and is correct before deleting
- **Use fs_read_file first**: Review content if unsure about deletion
- **No confirmation required**: Unlike fs_delete_directory, executes immediately
