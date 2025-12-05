---
name: memory-memorize
description: >
  Store content in a named memory library with automatic embedding generation for semantic search.
  WHEN: User says "remember this", "save for later", "store this knowledge", "add to my notes", "memorize this", "keep this information".
  WHEN NOT: Looking up previously stored info (use memory_recall), listing libraries (use memory_list_libraries).
version: 0.1.0
---

# Memory Memorize

## Core Concept

`mcp__plugin_kg_kodegen__memory_memorize` stores content in a named memory library with automatic vector embedding generation. The stored memory can later be retrieved using semantic search via `memory_recall`.

**How It Works**:
1. Content is processed and embedded into a vector representation
2. The embedding captures semantic meaning (not just keywords)
3. Memory is tagged with the library name for namespace isolation
4. Later, `memory_recall` finds semantically similar content

**Library Namespace Pattern**: Each library is a separate namespace. Choose library names that logically group related knowledge.

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `library` | string | Yes | Library name (namespace) to store the memory in |
| `content` | string | Yes | Content to memorize |

## Usage Examples

### Store a Code Pattern
```json
{
  "library": "rust-patterns",
  "content": "Error handling pattern: Use `thiserror` for library errors and `anyhow` for application errors. Define custom error types with #[derive(Error, Debug)] and implement Display for user-friendly messages."
}
```

### Store Meeting Notes
```json
{
  "library": "meeting-notes",
  "content": "2024-01-15 Architecture Review: Decided to use event sourcing for the order service. Key concerns: eventual consistency handling, event schema versioning. Action items: POC by Jan 30."
}
```

### Store API Documentation
```json
{
  "library": "api-reference",
  "content": "POST /users endpoint: Creates a new user. Required fields: email, name. Optional: phone, address. Returns 201 with user object on success, 400 for validation errors, 409 if email exists."
}
```

### Store a Code Snippet
```json
{
  "library": "snippets-typescript",
  "content": "TypeScript retry with exponential backoff:\n```typescript\nasync function retry<T>(fn: () => Promise<T>, maxAttempts = 3): Promise<T> {\n  for (let i = 0; i < maxAttempts; i++) {\n    try { return await fn(); }\n    catch (e) { if (i === maxAttempts - 1) throw e; await sleep(2 ** i * 1000); }\n  }\n  throw new Error('Unreachable');\n}\n```"
}
```

## Best Practices for Content

### DO: Include Context
```json
{
  "library": "project-notes",
  "content": "Authentication flow for Project X: Users authenticate via OAuth2 with Google. Tokens stored in HTTP-only cookies. Refresh happens automatically via middleware. Session timeout: 24 hours."
}
```

### DON'T: Store Raw Data Without Context
```json
{
  "library": "project-notes",
  "content": "OAuth2, Google, cookies, 24h"
}
```

### Chunking Strategy
- **Single concepts**: Store one idea/pattern/note per memory
- **Searchable context**: Include keywords that help semantic search
- **Self-contained**: Each memory should be understandable on its own

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Store new knowledge | memory_memorize | Creates embedding for semantic search |
| Find stored memories | memory_recall | Semantic search across library |
| List available libraries | memory_list_libraries | Discover namespaces |
| Update a memory | memory_memorize | Store new version (old remains) |

## Remember

- **Embeddings are automatic**: No manual vectorization needed
- **Library is namespace**: Choose meaningful, consistent library names
- **Content should be self-contained**: Include enough context for understanding
- **Semantic search later**: Content is found by meaning, not exact keywords
- **Libraries are created on first use**: No need to pre-create libraries
- **Memories are additive**: Storing doesn't replace previous memories
