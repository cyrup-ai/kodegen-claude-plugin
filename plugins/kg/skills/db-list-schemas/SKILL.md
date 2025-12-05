---
name: db-list-schemas
description: >
  List all databases or schemas available on the connected database server.
  WHEN: User asks "what schemas exist", "what databases are available", "show me the database structure", or when starting to explore an unfamiliar database.
  WHEN NOT: Looking for specific tables (use db_list_tables), need column details (use db_table_schema), or want to query data (use db_execute_sql).
version: 0.1.0
---

# db_list_schemas Mastery Guide

## Core Concept

`mcp__plugin_kg_kodegen__db_list_schemas` lists all databases or schemas available on the connected server. This is typically the **first step** when exploring an unfamiliar database - start here to discover what schemas exist before diving into tables.

## Key Parameters

This tool takes no parameters - just call it with an empty object.

```json
{}
```

## Usage Examples

### List All Schemas
```json
{}
```

**Response:**
```json
{
  "schemas": [
    {"name": "public", "type": "schema"},
    {"name": "information_schema", "type": "schema"},
    {"name": "pg_catalog", "type": "schema"}
  ]
}
```

## Database-Specific Behavior

| Database | Returns |
|----------|---------|
| PostgreSQL | Schema names (public, pg_catalog, etc.) |
| MySQL/MariaDB | Database names |
| SQLite | Returns "main" (single database) |

## Schema Exploration Workflow

```
1. db_list_schemas({})           → See available schemas
2. db_list_tables({schema: X})   → See tables in chosen schema
3. db_table_schema({schema, table}) → See column details
4. db_execute_sql({sql: "..."})  → Query the data
```

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| See available schemas/databases | db_list_schemas | Purpose-built for schema discovery |
| See tables in a schema | db_list_tables | Requires schema name from this tool |
| See column structure | db_table_schema | Need schema + table from above tools |
| Query data | db_execute_sql | After understanding schema structure |

## Remember

- **Start here** when exploring an unfamiliar database
- **No parameters needed** - just pass empty object `{}`
- **PostgreSQL** returns schemas (public, etc.)
- **MySQL/MariaDB** returns databases
- **SQLite** always returns "main"
- **Next step** is usually `db_list_tables` with the schema name you discovered
