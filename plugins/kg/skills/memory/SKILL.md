---
name: memory
description: >
  Memory system with three MCP tools: mcp__plugin_kg_kodegen__memory_list_libraries (discover namespaces), mcp__plugin_kg_kodegen__memory_memorize (store with embeddings), mcp__plugin_kg_kodegen__memory_recall (semantic search). Use for storing/retrieving knowledge by meaning.
  WHEN: User says "remember this", "recall", "what did I save", "my notes", "find my knowledge about".
  WHEN NOT: File operations (use fs_*), exact keyword search (use fs_search).
---

# Memory System

## Three MCP Tools

### 1. mcp__plugin_kg_kodegen__memory_list_libraries
**Purpose**: List all memory library names (namespaces)  
**Parameters**: None - use `{}`  
**Returns**: Array of library names

### 2. mcp__plugin_kg_kodegen__memory_memorize
**Purpose**: Store content with automatic embeddings  
**Parameters**:
- `library` (string, required) - Namespace
- `content` (string, required) - What to store

### 3. mcp__plugin_kg_kodegen__memory_recall
**Purpose**: Semantic search by meaning (not keywords)  
**Parameters**:
- `library` (string, required) - Which namespace to search
- `context` (string, required) - Natural language query
- `limit` (number, optional, default: 10) - Max results

## When to Use Each

| User Says | Tool | Why |
|-----------|------|-----|
| "remember this", "save this" | mcp__plugin_kg_kodegen__memory_memorize | Store new knowledge |
| "what did I save about X" | mcp__plugin_kg_kodegen__memory_recall | Find by semantic meaning |
| "what libraries exist" | mcp__plugin_kg_kodegen__memory_list_libraries | Discover namespaces |

## Key Concepts

**Semantic Search**: Finds by *meaning*, not exact words. "authentication flow" matches "login process", "OAuth", "user sign-in" because semantically related.

**Libraries as Namespaces**: Each library is isolated. Use patterns like:
- `project-{name}` for project knowledge
- `snippets-{lang}` for code snippets
- `notes-{context}` for contextual notes

**Content Best Practices**:
- Include context, not just raw data
- One concept per memory
- Self-contained (understandable alone)
- Natural language with searchable terms

## Common Workflows

**Store**: 
```json
{"library": "project-notes", "content": "Authentication uses OAuth2 with Google. Tokens in HTTP-only cookies, 24h timeout."}
```

**Discover then Search**:
```json
// 1. List libraries
{}

// 2. Search specific library
{"library": "project-notes", "context": "authentication decisions", "limit": 5}
```

**Cross-library Search**: Use mcp__plugin_kg_kodegen__memory_list_libraries, then mcp__plugin_kg_kodegen__memory_recall from each relevant library.

## Query Tips

✅ **DO**: "async HTTP requests with retry and timeout"  
❌ **DON'T**: "http" or "retry"

Use natural language, describe what you're looking for. More context = better results.

## Remember

- Libraries created on first use (no pre-creation needed)
- Memories are additive (storing doesn't replace)
- Embeddings are automatic
- Results ranked by semantic similarity
