---
name: process-kill
description: >
  Terminate a running process by PID using SIGKILL.
  WHEN: Need to stop a stuck process, kill runaway process, cleanup orphaned processes.
  WHEN NOT: Just checking what's running (use process_list first to find PID).
version: 0.1.0
---

# Process Kill - Forceful Process Termination

## Core Concept

`mcp__plugin_kg_kodegen__process_kill` terminates a process by PID using SIGKILL signal. This is a **forceful termination** - the process gets no chance for graceful shutdown or cleanup.

## Key Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `pid` | number | Yes | Process ID to terminate |

## Usage Examples

### Kill Process by PID
```json
{"pid": 12345}
```

### Output
```json
{
  "success": true,
  "pid": 12345,
  "message": "Successfully terminated process 12345"
}
```

## Common Workflows

### Find and Kill Hung Process
```json
// Step 1: Find the hung process
// Use process_list: {"filter": "python", "limit": 5}
// Returns: [{"pid": 23456, "name": "python", "cpu_percent": 99.8}]

// Step 2: Kill it
{"pid": 23456}

// Step 3: Verify termination
// Use process_list: {"filter": "python"}
// Returns: {"count": 0, "processes": []}
```

### Cleanup Multiple Orphaned Processes
```json
// For each PID from process_list results:
{"pid": 1111}
{"pid": 2222}
{"pid": 3333}
```

## Error Cases

### Process Not Found
```json
// Trying to kill non-existent process
{"pid": 99999}
// Error: "Failed to kill process 99999: Process not found"
```

### Permission Denied
```json
// Trying to kill protected process (e.g., init)
{"pid": 1}
// Error: "Failed to kill process 1: Permission denied or process protected"
```

## Warnings

| Warning | Description |
|---------|-------------|
| **SIGKILL is forceful** | No graceful shutdown - unsaved data will be lost |
| **NOT idempotent** | Second kill of same PID fails with "Process not found" |
| **Children may orphan** | Killing parent may leave child processes |
| **Always verify PID first** | Use process_list before killing |

## When to Use

| Scenario | Action |
|----------|--------|
| Hung/stuck process | Find PID with process_list, then kill |
| Runaway CPU consumer | Identify with process_list (high cpu_percent), then kill |
| Cleanup after tests | Kill leftover test processes |
| Stop background daemon | Kill after graceful shutdown fails |

## Remember

- **Always use process_list first** - verify the correct PID
- **SIGKILL = no graceful shutdown** - process cannot cleanup
- **Unsaved data is lost** - warn user if relevant
- **Handle "not found" gracefully** - process may have already exited
- **Check for child processes** - may need to kill those too
- **Not idempotent** - don't retry on same PID
