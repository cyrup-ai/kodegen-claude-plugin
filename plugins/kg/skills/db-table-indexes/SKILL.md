---
name: db-table-indexes
description: >
  Get index information for a database table including primary keys, unique constraints, and secondary indexes.
  WHEN: User asks about indexes, needs performance tuning info, wants to optimize queries, asks "what indexes exist", or needs to understand primary/foreign keys.
  WHEN NOT: Need column details (use db_table_schema), need to list tables (use db_list_tables), or need to query data (use db_execute_sql).
version: 0.1.0
---

# db_table_indexes Mastery Guide

## Core Concept

`mcp__plugin_kg_kodegen__db_table_indexes` retrieves index information for a specific table, including primary keys, unique constraints, and secondary indexes. Essential for query optimization and understanding table relationships.

## Key Parameters

### schema (required)
The schema or database name containing the table.

### table (required)
The table name to get indexes for.

```json
{
  "schema": "public",
  "table": "employees"
}
```

## Usage Examples

### Get Indexes for Employees Table
```json
{
  "schema": "public",
  "table": "employees"
}
```

**Response:**
```json
{
  "indexes": [
    {
      "name": "employees_pkey",
      "columns": ["id"],
      "unique": true,
      "primary": true
    },
    {
      "name": "idx_employee_department",
      "columns": ["department_id"],
      "unique": false,
      "primary": false
    },
    {
      "name": "idx_employee_email",
      "columns": ["email"],
      "unique": true,
      "primary": false
    },
    {
      "name": "idx_employee_name_dept",
      "columns": ["name", "department_id"],
      "unique": false,
      "primary": false
    }
  ]
}
```

## Understanding Index Info

| Field | Meaning |
|-------|---------|
| name | Index name |
| columns | Columns in the index (order matters for composite) |
| unique | true = enforces uniqueness |
| primary | true = this is the primary key |

## Query Optimization Tips

### Indexes Speed Up Queries On:
- **WHERE clauses**: `WHERE department_id = 1` (if indexed)
- **JOIN conditions**: `ON e.department_id = d.id` (if indexed)
- **ORDER BY**: `ORDER BY name` (if indexed)
- **Composite indexes**: Match column order for best performance

### Example: Writing Optimized Queries
```json
// Assuming idx_employee_department exists on department_id
{
  "sql": "SELECT * FROM employees WHERE department_id = 1",
  "readonly": true
}
// This query will use the index for fast lookup
```

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Column details | db_table_schema | Data types, nullability |
| Index/key info | db_table_indexes | This tool - performance info |
| Query optimization | db_table_indexes | Know what's indexed |
| Primary key | db_table_indexes | Look for primary: true |
| Unique constraints | db_table_indexes | Look for unique: true |

## Remember

- **Primary key** is marked with `primary: true`
- **Unique constraints** have `unique: true`
- **Composite indexes** list multiple columns - order matters
- **Use for query optimization** - query on indexed columns for speed
- **JOIN columns** should usually be indexed
- **WHERE/ORDER BY columns** benefit from indexes
