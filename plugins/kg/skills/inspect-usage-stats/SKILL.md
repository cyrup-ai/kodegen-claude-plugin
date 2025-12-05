---
name: inspect-usage-stats
description: >
  Get tool usage statistics including success rates, call counts, and performance metrics.
  WHEN: User asks "usage stats", "how much have I used", "success rate", "what tools are being used", performance analysis.
  WHEN NOT: Viewing tool call history (use inspect_tool_calls).
version: 0.1.0
---

# inspect_usage_stats - Usage Statistics

## Core Concept

`mcp__plugin_kg_kodegen__inspect_usage_stats` provides aggregated statistics about tool usage for the current session. Useful for debugging, performance analysis, and understanding workflow patterns.

## Key Parameters

No required parameters. Call with empty object:

```json
{}
```

## Response Format

```json
{
  "success": true,
  "total_calls": 150,
  "tools_used": 12,
  "tool_usage": [
    { "tool_name": "fs_read_file", "call_count": 45 },
    { "tool_name": "fs_search", "call_count": 30 },
    { "tool_name": "terminal", "call_count": 25 }
  ],
  "session_duration_ms": 3600000,
  "success_rate": 95.3,
  "successful_calls": 143,
  "failed_calls": 7
}
```

## Key Metrics

| Metric | Description |
|--------|-------------|
| `total_calls` | Total tool invocations this session |
| `tools_used` | Number of distinct tools used |
| `success_rate` | Percentage of successful calls |
| `successful_calls` | Count of successful calls |
| `failed_calls` | Count of failed calls |
| `session_duration_ms` | Time since session started |
| `tool_usage` | Per-tool breakdown |

## Interpreting Results

### Success Rate Thresholds
- **> 90%**: Healthy workflow
- **80-90%**: Some issues, investigate
- **< 80%**: Significant problems

### Warning Signs
- Low success rate on specific tools
- High failure counts in one category
- Sudden drops during session

### Common Failure Causes
- Input validation errors (wrong paths, invalid arguments)
- Expected failures (file doesn't exist checks)
- Permission issues
- Missing dependencies

## Use Cases

1. **Mid-session debugging** - Workflows seem slow or unreliable
2. **End-of-session review** - Understand what was accomplished
3. **Performance analysis** - Identify heavily-used tool categories
4. **Failure investigation** - Spot problematic tools

## Remember

- Statistics are session-scoped (reset on server restart)
- Failures don't always indicate bugs - may be expected behavior
- Use alongside `inspect_tool_calls` for detailed investigation
- Check success rate trends during complex workflows
- Category breakdown shows workflow patterns
