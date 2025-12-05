---
name: fs-edit-block
description: >
  Apply surgical text replacements to files using exact string matching.
  WHEN: User wants to change specific text, fix a line, update a value, replace a string, modify code precisely.
  WHEN NOT: Rewriting entire file (use fs_write_file), searching files (use fs_search), reading files (use fs_read_file).
version: 0.1.0
---

# fs_edit_block Guide

## Core Concept

`mcp__plugin_kg_kodegen__fs_edit_block` performs surgical text replacement in files. It finds an exact string (`old_string`) and replaces it with a new string (`new_string`). This is the preferred tool for precise code modifications without rewriting entire files.

## Key Parameters

| Parameter | Required | Default | Description |
|-----------|----------|---------|-------------|
| `path` | Yes | - | Path to the file to edit |
| `old_string` | Yes | - | Exact text to find and replace |
| `new_string` | Yes | - | Replacement text |
| `expected_replacements` | No | 1 | Expected number of replacements (safety check) |

## Usage Examples

### Replace a Function Name
```json
{
  "path": "/project/src/utils.ts",
  "old_string": "function oldName(",
  "new_string": "function newName("
}
```

### Update a Version Number
```json
{
  "path": "/project/package.json",
  "old_string": "\"version\": \"1.0.0\"",
  "new_string": "\"version\": \"1.1.0\""
}
```

### Fix a Typo in Multiple Places
```json
{
  "path": "/project/README.md",
  "old_string": "recieve",
  "new_string": "receive",
  "expected_replacements": 3
}
```

### Replace a Multi-line Block
```json
{
  "path": "/project/src/config.ts",
  "old_string": "const config = {\n  debug: true,\n  port: 3000\n}",
  "new_string": "const config = {\n  debug: false,\n  port: 8080\n}"
}
```

### Delete Code (Replace with Empty)
```json
{
  "path": "/project/src/app.ts",
  "old_string": "console.log('debug');",
  "new_string": ""
}
```

## The `expected_replacements` Safety Mechanism

Use this to ensure you're replacing exactly what you intend:

```json
// Expect exactly 1 replacement (default)
{
  "path": "/file.txt",
  "old_string": "foo",
  "new_string": "bar"
}

// Expect multiple replacements
{
  "path": "/file.txt",
  "old_string": "TODO",
  "new_string": "DONE",
  "expected_replacements": 5
}
```

**If the actual count doesn't match `expected_replacements`, the operation fails** - protecting against unintended changes.

## Common Patterns

### Add Import Statement
```json
{
  "path": "/project/src/component.tsx",
  "old_string": "import React from 'react';",
  "new_string": "import React from 'react';\nimport { useState } from 'react';"
}
```

### Modify Function Body
```json
{
  "path": "/project/src/api.ts",
  "old_string": "return fetch(url);",
  "new_string": "return fetch(url, { credentials: 'include' });"
}
```

### Update Configuration Value
```json
{
  "path": "/project/.env",
  "old_string": "API_URL=http://localhost:3000",
  "new_string": "API_URL=https://api.example.com"
}
```

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Change specific text | fs_edit_block | Precise, surgical replacement |
| Rewrite entire file | fs_write_file | Complete file replacement |
| Add to end of file | fs_write_file (mode: append) | Append without finding text |
| Find text first | fs_search | Locate before editing |
| View current content | fs_read_file | Check before modifying |

## Troubleshooting

### "String not found" Error
1. **Check exact whitespace**: Spaces, tabs, newlines must match exactly
2. **Check line endings**: Unix (LF) vs Windows (CRLF)
3. **Use fs_read_file first**: Copy exact text from file content

### "Unexpected replacement count"
1. The string appears more/fewer times than expected
2. Adjust `expected_replacements` or make `old_string` more specific

## Remember

- **Exact match required**: `old_string` must match character-for-character
- **Whitespace matters**: Include exact spaces, tabs, newlines
- **Use `expected_replacements`**: Safety check for precise operations
- **Multi-line support**: Include `\n` for newlines in JSON strings
- **Read file first**: Use fs_read_file to verify exact text before editing
- **Prefer over fs_write_file**: For small changes, surgical edits are safer
