---
name: db-list-tables
description: >
  List all tables within a specific database schema.
  WHEN: User asks "what tables exist", "show me the tables", "list tables in schema X", or exploring tables after identifying a schema.
  WHEN NOT: Need column/constraint details (use db_table_schema), need to query data (use db_execute_sql), or need index info (use db_table_indexes).
version: 0.1.0
---

# db_list_tables Mastery Guide

## Core Concept

`mcp__plugin_kg_kodegen__db_list_tables` lists all tables within a specific schema or database. Use this after `db_list_schemas` to discover what tables are available before diving into column details.

## Key Parameters

### schema (required for PostgreSQL/MySQL, optional for SQLite)
The schema or database name to list tables from.

```json
{"schema": "public"}
```

## Usage Examples

### List Tables in Public Schema (PostgreSQL)
```json
{
  "schema": "public"
}
```

**Response:**
```json
{
  "tables": [
    {"name": "employees", "type": "table"},
    {"name": "departments", "type": "table"},
    {"name": "projects", "type": "table"},
    {"name": "employee_view", "type": "view"}
  ]
}
```

### List Tables in MySQL Database
```json
{
  "schema": "myapp_production"
}
```

### List Tables in SQLite
```json
{}
```
SQLite has a single database, so schema is optional.

## Schema Exploration Workflow

```
1. db_list_schemas({})              → ["public", "analytics"]
2. db_list_tables({schema: "public"})  → ["employees", "departments", ...]
3. db_table_schema({schema: "public", table: "employees"})
4. db_execute_sql({sql: "SELECT * FROM employees LIMIT 10"})
```

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| List schemas/databases | db_list_schemas | First step in exploration |
| List tables in schema | db_list_tables | This tool - after knowing schema |
| See column details | db_table_schema | After identifying table of interest |
| See indexes | db_table_indexes | For query optimization |
| Query data | db_execute_sql | After understanding structure |

## Remember

- **Requires schema name** (get it from `db_list_schemas` first)
- **Returns both tables and views** - check the "type" field
- **PostgreSQL** uses "public" as default schema
- **MySQL/MariaDB** schema = database name
- **SQLite** schema is optional (single database)
- **Next step** is usually `db_table_schema` for column details
