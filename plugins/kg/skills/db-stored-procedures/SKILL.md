---
name: db-stored-procedures
description: >
  List stored procedures and functions available in the database (PostgreSQL and MySQL only).
  WHEN: User asks "what stored procedures exist", "show me available functions", "what procedures can I call", or exploring database capabilities.
  WHEN NOT: Need to execute a procedure (use db_execute_sql with CALL/SELECT), exploring tables (use db_list_tables), or SQLite database (not supported).
version: 0.1.0
---

# db_stored_procedures Mastery Guide

## Core Concept

`mcp__plugin_kg_kodegen__db_stored_procedures` lists stored procedures and functions available in a database schema. **Only supported on PostgreSQL and MySQL/MariaDB** - SQLite does not support stored procedures.

## Key Parameters

### schema (required)
The schema or database name to list procedures from.

```json
{
  "schema": "public"
}
```

## Usage Examples

### List Procedures in Public Schema (PostgreSQL)
```json
{
  "schema": "public"
}
```

**Response:**
```json
{
  "procedures": [
    {
      "name": "get_department_employee_count",
      "schema": "public",
      "return_type": "integer",
      "language": "plpgsql"
    },
    {
      "name": "update_employee_salary",
      "schema": "public",
      "return_type": "void",
      "language": "plpgsql"
    },
    {
      "name": "calculate_bonus",
      "schema": "public",
      "return_type": "numeric",
      "language": "sql"
    }
  ]
}
```

### List Procedures in MySQL Database
```json
{
  "schema": "myapp_db"
}
```

## Understanding Procedure Info

| Field | Meaning |
|-------|---------|
| name | Procedure/function name |
| schema | Schema containing the procedure |
| return_type | Return data type (void = no return) |
| language | Implementation language (plpgsql, sql, etc.) |

## Executing Stored Procedures

After discovering procedures, execute them with `db_execute_sql`:

### PostgreSQL Function Call
```json
{
  "sql": "SELECT get_department_employee_count(1)",
  "readonly": true
}
```

### PostgreSQL Procedure Call
```json
{
  "sql": "CALL update_employee_salary(123, 75000)",
  "readonly": false
}
```

### MySQL Procedure Call
```json
{
  "sql": "CALL get_department_stats(1)",
  "readonly": true
}
```

## Database Support

| Database | Stored Procedures |
|----------|-------------------|
| PostgreSQL | Full support (functions + procedures) |
| MySQL | Full support |
| MariaDB | Full support |
| SQLite | NOT SUPPORTED |

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| List procedures | db_stored_procedures | This tool - discovery |
| Execute procedure | db_execute_sql | Use CALL or SELECT syntax |
| List tables | db_list_tables | Different purpose |
| Schema exploration | db_list_schemas | Start of exploration |

## Remember

- **PostgreSQL/MySQL only** - SQLite does not support stored procedures
- **Discovery tool** - lists available procedures, doesn't execute them
- **Execute with db_execute_sql** using CALL (procedures) or SELECT (functions)
- **Check return_type** - "void" means procedure, others are functions
- **Check language** - shows implementation language (plpgsql, sql, etc.)
