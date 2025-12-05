---
name: memory-list-libraries
description: >
  List all unique memory library names that have been created. Discover what knowledge namespaces exist.
  WHEN: User asks "what libraries exist", "what have I saved", "show my memories", "list knowledge bases", "what namespaces are available".
  WHEN NOT: Looking for specific memories (use memory_recall), storing new info (use memory_memorize).
version: 0.1.0
---

# Memory List Libraries

## Core Concept

`mcp__plugin_kg_kodegen__memory_list_libraries` returns a list of all memory library names that contain at least one stored memory. Libraries are namespaces that organize memories into logical groups.

**Library Namespace Pattern**: Each library is an isolated namespace. Memories stored in one library are only searchable within that library. This allows organizing knowledge by:
- Project (e.g., "project-alpha", "backend-api")
- Topic (e.g., "rust-patterns", "sql-queries")
- Context (e.g., "meeting-notes", "code-reviews")
- User (e.g., "personal", "team-shared")

## Parameters

This tool takes no parameters.

```json
{}
```

## Usage Examples

### List All Libraries
```json
{}
```

Returns array of library names:
```json
["project-notes", "code-snippets", "api-docs", "meeting-summaries"]
```

### Common Workflow: Discover Then Recall
```
1. Use memory_list_libraries to see available namespaces
2. Identify the relevant library
3. Use memory_recall with that library to search for specific memories
```

## Library Naming Conventions

| Pattern | Example | Use Case |
|---------|---------|----------|
| `project-{name}` | `project-kodegen` | Project-specific knowledge |
| `topic-{subject}` | `topic-rust` | Subject matter expertise |
| `notes-{context}` | `notes-meetings` | Contextual notes |
| `snippets-{lang}` | `snippets-python` | Code snippets by language |

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| See available libraries | memory_list_libraries | Lists all namespaces |
| Search for memories | memory_recall | Semantic search within a library |
| Store new knowledge | memory_memorize | Add to a library |
| Check if library exists | memory_list_libraries | Verify before recall |

## Remember

- **No parameters needed**: Just invoke with empty object `{}`
- **Returns library names only**: Not the memories themselves
- **Only non-empty libraries**: Libraries with no memories are not listed
- **Use before recall**: Verify library exists before searching
- **Namespace isolation**: Each library is independent
