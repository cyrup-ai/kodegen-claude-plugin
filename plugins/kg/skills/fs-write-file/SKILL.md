---
name: fs-write-file
description: >
  Write or append content to files. Creates new files or overwrites/appends to existing ones.
  WHEN: User wants to create a file, write content, save file, overwrite file, append to file, add to log.
  WHEN NOT: Small precise edits (use fs_edit_block), reading files (use fs_read_file).
version: 0.1.0
---

# fs_write_file Guide

## Core Concept

`mcp__plugin_kg_kodegen__fs_write_file` writes content to a file. It supports two modes: `rewrite` (overwrite entire file) and `append` (add to end). Creates the file if it doesn't exist. Creates parent directories automatically.

## Key Parameters

| Parameter | Required | Default | Description |
|-----------|----------|---------|-------------|
| `path` | Yes | - | Path to the file to write |
| `content` | Yes | - | Content to write |
| `mode` | No | "rewrite" | `"rewrite"` or `"append"` |

## Usage Examples

### Create New File
```json
{
  "path": "/project/src/newfile.ts",
  "content": "export const greeting = 'Hello, World!';\n"
}
```

### Overwrite Existing File
```json
{
  "path": "/project/config.json",
  "content": "{\n  \"name\": \"myapp\",\n  \"version\": \"2.0.0\"\n}",
  "mode": "rewrite"
}
```

### Append to File
```json
{
  "path": "/project/CHANGELOG.md",
  "content": "\n## v1.1.0\n- Added new feature\n- Fixed bug\n",
  "mode": "append"
}
```

### Append to Log
```json
{
  "path": "/project/logs/debug.log",
  "content": "2024-01-15 10:30:00 - Debug message\n",
  "mode": "append"
}
```

## Mode Comparison

| Mode | Behavior | Use Case |
|------|----------|----------|
| `rewrite` | Replaces entire file | New files, complete rewrites |
| `append` | Adds to end of file | Logs, changelogs, growing files |

## Creating Multi-Line Files

Use `\n` for newlines in JSON:

```json
{
  "path": "/project/src/types.ts",
  "content": "export interface User {\n  id: string;\n  name: string;\n  email: string;\n}\n\nexport interface Post {\n  id: string;\n  title: string;\n  content: string;\n}\n"
}
```

Or use the natural multi-line format if supported:

```json
{
  "path": "/project/README.md",
  "content": "# My Project\n\nThis is a sample project.\n\n## Installation\n\n```bash\nnpm install\n```\n"
}
```

## Common Patterns

### Create Configuration File
```json
{
  "path": "/project/.env",
  "content": "DATABASE_URL=postgresql://localhost:5432/mydb\nAPI_KEY=your-api-key\nDEBUG=true\n"
}
```

### Create TypeScript Module
```json
{
  "path": "/project/src/utils/format.ts",
  "content": "export function formatDate(date: Date): string {\n  return date.toISOString().split('T')[0];\n}\n\nexport function formatCurrency(amount: number): string {\n  return new Intl.NumberFormat('en-US', {\n    style: 'currency',\n    currency: 'USD'\n  }).format(amount);\n}\n"
}
```

### Add Entry to Existing File
```json
{
  "path": "/project/src/index.ts",
  "content": "export * from './newModule';\n",
  "mode": "append"
}
```

## fs_write_file vs fs_edit_block

| Scenario | Best Tool | Why |
|----------|-----------|-----|
| Create new file | fs_write_file | Writes complete content |
| Replace entire file | fs_write_file | Complete rewrite |
| Change specific line | fs_edit_block | Surgical precision |
| Fix typo | fs_edit_block | Minimal change |
| Add to end of file | fs_write_file (append) | Dedicated append mode |
| Insert in middle | fs_edit_block | Find and replace |

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Create new file | fs_write_file | Writes content to new path |
| Full file rewrite | fs_write_file | Replace all content |
| Append to file | fs_write_file (append) | Add without reading |
| Change specific text | fs_edit_block | Surgical replacement |
| Read before write | fs_read_file | Get current content |
| Create directory | fs_create_directory | Containers only |

## Remember

- **Two modes**: `rewrite` (default) overwrites, `append` adds to end
- **Creates files**: No need to check if file exists first
- **Creates parent dirs**: Parent directories created automatically
- **Use fs_edit_block for changes**: For existing file modifications
- **Escape special chars**: Use `\n` for newlines, `\"` for quotes in JSON
- **Full content for rewrite**: Mode `rewrite` replaces everything
