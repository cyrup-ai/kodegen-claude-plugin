---
name: db-pool-stats
description: >
  Get database connection pool statistics for monitoring and debugging.
  WHEN: User asks about database connections, pool health, debugging connection issues, monitoring database performance, or troubleshooting "connection refused" errors.
  WHEN NOT: Need to query data (use db_execute_sql), exploring schema (use db_list_schemas), or checking table structure (use db_table_schema).
version: 0.1.0
---

# db_pool_stats Mastery Guide

## Core Concept

`mcp__plugin_kg_kodegen__db_pool_stats` retrieves connection pool statistics for monitoring database connection health. Useful for diagnosing connection issues, monitoring pool utilization, and performance troubleshooting.

## Key Parameters

This tool takes no parameters - just call it with an empty object.

```json
{}
```

## Usage Examples

### Get Pool Statistics
```json
{}
```

**Response:**
```json
{
  "connections": 5,
  "idle_connections": 3,
  "max_connections": 10,
  "min_connections": 2,
  "wait_queue_size": 0
}
```

## Understanding Pool Stats

| Metric | Meaning | Healthy Range |
|--------|---------|---------------|
| connections | Current total connections | < max_connections |
| idle_connections | Available connections | > 0 ideally |
| max_connections | Pool maximum size | Configured limit |
| min_connections | Pool minimum size | Warm connections kept |
| wait_queue_size | Queries waiting for connection | 0 is ideal |

## Diagnosing Issues

### High wait_queue_size
```json
{"wait_queue_size": 15}  // PROBLEM: queries waiting
```
**Cause**: Not enough connections for query load
**Fix**: Increase `db_max_connections` in config

### connections == max_connections with wait_queue > 0
```json
{
  "connections": 10,
  "max_connections": 10,
  "wait_queue_size": 5
}
```
**Cause**: Pool exhausted
**Fix**: Increase max_connections or optimize slow queries

### idle_connections == 0
```json
{"idle_connections": 0, "connections": 10}
```
**Cause**: All connections in use
**Status**: Normal under load, concerning if persistent

## Pool Configuration

Configure via KODEGEN settings:
```json
{
  "db_min_connections": 2,
  "db_max_connections": 10,
  "db_acquire_timeout_secs": 30,
  "db_idle_timeout_secs": 600,
  "db_max_lifetime_secs": 1800
}
```

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Check connection health | db_pool_stats | This tool |
| Debug "connection refused" | db_pool_stats | See if pool exhausted |
| Monitor performance | db_pool_stats | Track utilization |
| Query data | db_execute_sql | Different purpose |
| Explore schema | db_list_schemas | Different purpose |

## Remember

- **No parameters needed** - just pass empty object `{}`
- **Monitor wait_queue_size** - should be 0 normally
- **Check idle_connections** - should have some available
- **Pool exhaustion** = connections == max_connections + wait_queue > 0
- **Useful for debugging** connection timeouts and performance issues
- **Not for data queries** - use db_execute_sql for that
