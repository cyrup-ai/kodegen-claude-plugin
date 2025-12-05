---
name: inspect-tool-calls
description: >
  View history of tool calls with arguments and outputs for debugging and context recovery.
  WHEN: User asks "what tools were called", "show tool history", "what work was done", debugging, auditing, onboarding new chat.
  WHEN NOT: Checking usage statistics (use inspect_usage_stats).
version: 0.1.0
---

# inspect_tool_calls - Tool Call History

## Core Concept

`mcp__plugin_kg_kodegen__inspect_tool_calls` retrieves chronological history of tool executions. Useful for onboarding new chats about work done, debugging sequences, and recovering context after chat history loss.

## Key Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `max_results` | number | 50 | Maximum calls to return |
| `offset` | number | 0 | Pagination offset (negative = tail) |
| `tool_name` | string | null | Filter by specific tool |
| `since` | string | null | ISO timestamp filter |

## Usage Examples

### Get Recent History (default 50)
```json
{}
```

### Get Last 20 Calls (most recent)
```json
{ "offset": -20 }
```

### Paginate (calls 50-99)
```json
{ "offset": 50, "max_results": 50 }
```

### Filter by Tool
```json
{ "tool_name": "fs_read_file" }
```

### Filter by Tool + Recent
```json
{ "tool_name": "fs_search", "offset": -10 }
```

### Filter by Time
```json
{ "since": "2024-10-12T20:00:00Z" }
```

## Response Format

```json
{
  "success": true,
  "count": 10,
  "total_entries_in_memory": 150,
  "calls": [
    {
      "tool_name": "fs_read_file",
      "timestamp": "2024-10-12T20:15:30Z",
      "duration_ms": 45,
      "args_json": "{\"path\": \"/src/main.rs\"}",
      "output_json": "{\"success\": true, ...}"
    }
  ]
}
```

## Common Use Cases

1. **New chat context** - Understand what work was already done
2. **Debugging** - Trace sequence of operations
3. **Learning** - See how tools were combined for a task
4. **Auditing** - Review what actions were taken

## Pagination Behavior

- **Positive offset**: Start from beginning (oldest first)
- **Negative offset**: Start from end (most recent)
  - `offset: -20` = last 20 calls
  - `offset: -10, max_results: 5` = calls 10-6 from end

## Remember

- History kept in memory (last 1000 calls)
- Persisted to `~/.config/kodegen-mcp/tool-history.jsonl`
- Does not track its own calls (meta/query tools excluded)
- Use negative offset for "tail" behavior (most recent first)
- Combine `tool_name` filter with `offset: -N` for recent calls of specific tool
