---
name: db-table-schema
description: >
  Get detailed column information for a specific database table including data types, nullability, and default values.
  WHEN: User asks "describe table X", "what columns does X have", "show table structure", "what's the schema of X", or understanding data model before writing queries.
  WHEN NOT: Just need list of tables (use db_list_tables), need index info (use db_table_indexes), or need to query data (use db_execute_sql).
version: 0.1.0
---

# db_table_schema Mastery Guide

## Core Concept

`mcp__plugin_kg_kodegen__db_table_schema` retrieves detailed column information for a specific table, including data types, nullability constraints, and default values. Essential for understanding table structure before writing queries.

## Key Parameters

### schema (required)
The schema or database name containing the table.

### table (required)
The table name to describe.

```json
{
  "schema": "public",
  "table": "employees"
}
```

## Usage Examples

### Describe Employees Table
```json
{
  "schema": "public",
  "table": "employees"
}
```

**Response:**
```json
{
  "columns": [
    {
      "name": "id",
      "data_type": "integer",
      "nullable": false,
      "default": "nextval('employees_id_seq'::regclass)"
    },
    {
      "name": "name",
      "data_type": "varchar(255)",
      "nullable": false,
      "default": null
    },
    {
      "name": "email",
      "data_type": "varchar(255)",
      "nullable": false,
      "default": null
    },
    {
      "name": "department_id",
      "data_type": "integer",
      "nullable": true,
      "default": null
    },
    {
      "name": "salary",
      "data_type": "numeric(10,2)",
      "nullable": true,
      "default": null
    },
    {
      "name": "hire_date",
      "data_type": "date",
      "nullable": false,
      "default": "CURRENT_DATE"
    },
    {
      "name": "active",
      "data_type": "boolean",
      "nullable": false,
      "default": "true"
    }
  ]
}
```

### Describe MySQL Table
```json
{
  "schema": "myapp_db",
  "table": "orders"
}
```

## Understanding Column Info

| Field | Meaning |
|-------|---------|
| name | Column name |
| data_type | SQL data type (integer, varchar, boolean, etc.) |
| nullable | true = allows NULL, false = NOT NULL constraint |
| default | Default value or NULL if none |

## Schema Exploration Workflow

```
1. db_list_schemas({})                    → Find schema names
2. db_list_tables({schema: "public"})     → Find table names
3. db_table_schema({schema, table})       → THIS TOOL - understand columns
4. db_table_indexes({schema, table})      → Optional - check indexes
5. db_execute_sql({sql: "SELECT ..."})    → Write informed queries
```

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| List all tables | db_list_tables | Discovery, not details |
| Column details | db_table_schema | This tool - full column info |
| Index information | db_table_indexes | Performance/key analysis |
| Constraints/keys | db_table_indexes | Shows primary/unique keys |
| Query data | db_execute_sql | After understanding schema |

## Remember

- **Get schema/table names first** using `db_list_schemas` and `db_list_tables`
- **Shows all columns** with data types, nullability, defaults
- **Use before writing queries** to understand what data is available
- **Check nullable** to know which columns require values
- **Check data_type** to write correct SQL (quotes for strings, etc.)
- **Next step** is usually `db_execute_sql` to query the table
