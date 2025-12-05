---
name: fs-move-file
description: >
  Move or rename files and directories in a single operation.
  WHEN: User wants to rename a file, move file to different location, reorganize files, rename directory.
  WHEN NOT: Copying files (use fs_read_file + fs_write_file), deleting files (use fs_delete_file).
version: 0.1.0
---

# fs_move_file Guide

## Core Concept

`mcp__plugin_kg_kodegen__fs_move_file` moves or renames files and directories. It can move items between directories, rename them, or do both in a single operation. Works on both files and directories.

## Key Parameters

| Parameter | Required | Default | Description |
|-----------|----------|---------|-------------|
| `source` | Yes | - | Path to the file or directory to move |
| `destination` | Yes | - | Target path (new location and/or name) |

## Usage Examples

### Rename a File
```json
{
  "source": "/project/src/old-name.ts",
  "destination": "/project/src/new-name.ts"
}
```

### Move File to Different Directory
```json
{
  "source": "/project/temp/data.json",
  "destination": "/project/src/data/data.json"
}
```

### Move and Rename
```json
{
  "source": "/project/src/utils.ts",
  "destination": "/project/src/helpers/string-utils.ts"
}
```

### Rename a Directory
```json
{
  "source": "/project/src/old-feature",
  "destination": "/project/src/new-feature"
}
```

### Move Directory
```json
{
  "source": "/project/temp/components",
  "destination": "/project/src/components"
}
```

## Important Notes

### Destination Must Be Full Path
```json
// WRONG - destination must include filename
{
  "source": "/project/file.txt",
  "destination": "/project/backup/"  // Missing filename!
}

// CORRECT
{
  "source": "/project/file.txt",
  "destination": "/project/backup/file.txt"
}
```

### Parent Directory Must Exist
```json
// Fails if /project/new-folder doesn't exist
{
  "source": "/project/file.txt",
  "destination": "/project/new-folder/file.txt"
}

// Create parent first with fs_create_directory
```

## Common Patterns

### Organize Files
```json
// Move all config files to config folder
{"source": "/project/.eslintrc", "destination": "/project/config/.eslintrc"}
{"source": "/project/.prettierrc", "destination": "/project/config/.prettierrc"}
```

### Rename with Convention
```json
// kebab-case to PascalCase
{
  "source": "/project/src/my-component.tsx",
  "destination": "/project/src/MyComponent.tsx"
}
```

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Move or rename | fs_move_file | Single operation for relocation |
| Copy file | fs_read_file + fs_write_file | No native copy tool |
| Delete file | fs_delete_file | Permanent removal |
| Delete directory | fs_delete_directory | Recursive removal |
| Create directory | fs_create_directory | For new destination paths |

## Remember

- **Atomic operation**: Move and rename happen together
- **Works on directories**: Moves entire directory trees
- **Destination is full path**: Include filename, not just directory
- **Parent must exist**: Create destination directory first if needed
- **No copy**: This moves (removes from source), doesn't copy
- **Both paths required**: Must specify source and destination
