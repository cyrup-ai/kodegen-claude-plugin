---
name: process-list
description: >
  List running processes with CPU and memory statistics.
  WHEN: User asks "what's running", "check processes", debugging resource usage, finding runaway processes, identifying PIDs before killing.
  WHEN NOT: Need to actually kill a process (use process_kill after identifying PID).
version: 0.1.0
---

# Process List - Running Process Discovery

## Core Concept

`mcp__plugin_kg_kodegen__process_list` lists running processes with CPU and memory statistics. Results are automatically sorted by CPU usage (highest first), making it easy to identify resource-intensive processes.

## Key Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `filter` | string | No | None | Case-insensitive substring match on process name |
| `limit` | number | No | 0 | Maximum results (0 = unlimited) |

## Usage Examples

### List All Processes
```json
{}
```

### Find Specific Process
```json
{"filter": "python"}
```
Matches: python, python3, my-python-app, ipython

### Top 5 CPU Consumers
```json
{"limit": 5}
```

### Find Node.js Processes (Top 3)
```json
{"filter": "node", "limit": 3}
```

### Find Stuck Build Process
```json
{"filter": "cargo"}
```

## Output Format

```json
{
  "success": true,
  "count": 3,
  "processes": [
    {"pid": 1234, "name": "chrome", "cpu_percent": 45.2, "memory_mb": 512.3},
    {"pid": 5678, "name": "node", "cpu_percent": 23.1, "memory_mb": 256.8},
    {"pid": 9012, "name": "python", "cpu_percent": 12.5, "memory_mb": 128.4}
  ]
}
```

## Common Patterns

### Find and Kill Workflow
```json
// Step 1: Find the target process
{"filter": "webpack", "limit": 1}
// Returns: {"processes": [{"pid": 12345, "name": "webpack", "cpu_percent": 99.9}]}

// Step 2: Kill it (use process_kill tool)
// {"pid": 12345}

// Step 3: Verify it's gone
{"filter": "webpack"}
// Returns: {"count": 0, "processes": []}
```

### Debug Resource Usage
```json
// Find what's consuming CPU
{"limit": 10}

// Check specific application
{"filter": "vscode"}
```

## When to Use

| Scenario | Action |
|----------|--------|
| "What's using all my CPU?" | `{"limit": 5}` |
| "Is nginx running?" | `{"filter": "nginx"}` |
| "Find stuck process" | `{"filter": "<name>"}` |
| "List all processes" | `{}` |

## Remember

- **Results sorted by CPU** - highest consumers first
- **Filter is substring match** - "node" matches "node", "nodejs", "electron"
- **Case-insensitive** - "Python" matches "python", "PYTHON"
- **Use before process_kill** - always identify PID first
- **Limit for quick overview** - use limit for top N consumers
