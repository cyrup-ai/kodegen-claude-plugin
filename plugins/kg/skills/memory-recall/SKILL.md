---
name: memory-recall
description: >
  Retrieve relevant memories from a library using semantic search. Finds content by meaning using vector similarity.
  WHEN: User asks "what did I save about X", "recall notes on Y", "what do I know about Z", "find my notes on", "search my memories for".
  WHEN NOT: Storing new information (use memory_memorize), listing libraries (use memory_list_libraries).
version: 0.1.0
---

# Memory Recall

## Core Concept

`mcp__plugin_kg_kodegen__memory_recall` retrieves relevant memories from a library using semantic (vector) search. It finds content based on meaning similarity, not just keyword matching.

**How Semantic Search Works**:
1. Your search context is converted to a vector embedding
2. Cosine similarity compares it against stored memory embeddings
3. Most semantically similar memories are returned
4. Results are ranked by relevance score

**Key Insight**: You don't need exact keywords. "authentication flow" will find memories about "login process", "OAuth implementation", or "user sign-in" because they're semantically related.

## Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `library` | string | Yes | - | Library name to search in |
| `context` | string | Yes | - | Search query/context |
| `limit` | number | No | 10 | Maximum results to return |

## Usage Examples

### Basic Search
```json
{
  "library": "project-notes",
  "context": "authentication flow"
}
```

### Search with Limit
```json
{
  "library": "code-snippets",
  "context": "error handling retry logic",
  "limit": 5
}
```

### Semantic Query (No Exact Keywords Needed)
```json
{
  "library": "meeting-notes",
  "context": "decisions about the database architecture"
}
```
Finds meetings discussing "chose PostgreSQL", "database schema design", "data modeling choices", etc.

### Find Related Patterns
```json
{
  "library": "rust-patterns",
  "context": "how to handle errors gracefully"
}
```
Finds memories about `Result`, `?` operator, `thiserror`, `anyhow`, error propagation, etc.

## Search Query Best Practices

### DO: Use Natural Language
```json
{
  "library": "api-reference",
  "context": "how to create a new user account"
}
```

### DO: Be Specific About Intent
```json
{
  "library": "snippets-python",
  "context": "async HTTP requests with retry and timeout"
}
```

### DON'T: Use Single Keywords
```json
{
  "library": "notes",
  "context": "auth"
}
```
Better: "user authentication and authorization patterns"

### Query Formulation Tips
| Instead of... | Try... |
|---------------|--------|
| "db" | "database connection and query patterns" |
| "api" | "REST API endpoint implementation" |
| "test" | "unit testing strategies and mocking" |
| "error" | "error handling and recovery approaches" |

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Find stored memories | memory_recall | Semantic search by meaning |
| Store new knowledge | memory_memorize | Creates searchable memory |
| List available libraries | memory_list_libraries | Discover what's stored |
| Exact text search | fs_search | Keyword/regex in files |

## Remember

- **Semantic, not keyword**: Finds by meaning, not exact words
- **Library is required**: Must specify which namespace to search
- **Limit defaults to 10**: Adjust for more/fewer results
- **Natural language works**: Describe what you're looking for
- **Results are ranked**: Most relevant memories first
- **Context matters**: More descriptive queries yield better results
