---
name: fs-delete-directory
description: >
  Delete a directory and all its contents recursively. DESTRUCTIVE operation.
  WHEN: User wants to remove a folder, delete directory tree, clean up folders, remove project directories.
  WHEN NOT: Deleting a single file (use fs_delete_file), moving/renaming (use fs_move_file).
version: 0.1.0
---

# fs_delete_directory Guide

## Core Concept

`mcp__plugin_kg_kodegen__fs_delete_directory` removes a directory and ALL its contents recursively. This is a **permanent, destructive operation** that cannot be undone.

## WARNING

This operation is **PERMANENT** and **CANNOT BE UNDONE**. All files and subdirectories within the target directory will be deleted. Always verify the path before executing.

## Key Parameters

| Parameter | Required | Default | Description |
|-----------|----------|---------|-------------|
| `path` | Yes | - | Path to the directory to delete |
| `recursive` | Yes | false | **Must be `true`** to confirm deletion |

## Usage Examples

### Delete a Directory
```json
{
  "path": "/project/old-feature",
  "recursive": true
}
```

### Delete Build Artifacts
```json
{
  "path": "/project/target",
  "recursive": true
}
```

### Delete Node Modules
```json
{
  "path": "/project/node_modules",
  "recursive": true
}
```

## Safety Mechanism

The `recursive: true` parameter is a **safety confirmation**. Without it, the operation fails:

```json
// This FAILS - missing recursive confirmation
{
  "path": "/project/temp"
}

// This SUCCEEDS - explicit confirmation
{
  "path": "/project/temp",
  "recursive": true
}
```

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Delete directory tree | fs_delete_directory | Recursive deletion with confirmation |
| Delete single file | fs_delete_file | File-only deletion |
| Move directory elsewhere | fs_move_file | Non-destructive relocation |
| Check contents first | fs_list_directory | See what will be deleted |

## Remember

- **PERMANENT**: Cannot be undone - verify path carefully
- **Requires `recursive: true`**: Safety mechanism to prevent accidents
- **Deletes everything**: All files and subdirectories are removed
- **Use fs_list_directory first**: Check contents before deleting
- **Only for directories**: Use fs_delete_file for single files
