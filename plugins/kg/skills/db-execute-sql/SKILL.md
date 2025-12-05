---
name: db-execute-sql
description: >
  Execute SQL queries with transaction support, retry logic, and automatic row limiting.
  WHEN: User wants to query data, run SQL statements, INSERT/UPDATE/DELETE records, analyze data, run reports, or execute multi-statement transactions.
  WHEN NOT: Just exploring schema structure (use db_table_schema), listing tables (use db_list_tables), or checking indexes (use db_table_indexes).
version: 0.1.0
---

# db_execute_sql Mastery Guide

## Core Concept

`mcp__plugin_kg_kodegen__db_execute_sql` executes SQL queries against PostgreSQL, MySQL, MariaDB, or SQLite databases. It supports multi-statement transactions, automatic row limiting, and read-only mode enforcement for safe data exploration.

## Key Parameters

### sql (required)
The SQL statement(s) to execute. Supports multi-statement execution with transactions.

```json
{"sql": "SELECT * FROM employees WHERE department_id = 1"}
```

### readonly (default: false)
Enable read-only mode to prevent accidental data modifications. **Always use `true` for SELECT queries.**

```json
{"sql": "SELECT * FROM users", "readonly": true}
```

### max_rows (default: 1000)
Limit the number of rows returned. Prevents memory exhaustion on large result sets.

```json
{"sql": "SELECT * FROM large_table", "max_rows": 100}
```

## Usage Examples

### Basic SELECT Query
```json
{
  "sql": "SELECT id, name, email FROM employees WHERE active = true",
  "readonly": true,
  "max_rows": 50
}
```

### Analytical Query with Joins
```json
{
  "sql": "SELECT d.name as department, COUNT(e.id) as employee_count, AVG(e.salary) as avg_salary FROM departments d LEFT JOIN employees e ON d.id = e.department_id GROUP BY d.name ORDER BY employee_count DESC",
  "readonly": true,
  "max_rows": 100
}
```

### INSERT Statement
```json
{
  "sql": "INSERT INTO employees (name, email, department_id) VALUES ('Alice', 'alice@example.com', 1)",
  "readonly": false
}
```

### Multi-Statement Transaction
```json
{
  "sql": "INSERT INTO departments (name, budget) VALUES ('Engineering', 500000); INSERT INTO employees (name, email, department_id, salary) VALUES ('Bob', 'bob@example.com', 1, 85000);",
  "readonly": false
}
```

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Query data | db_execute_sql | Execute SELECT with row limiting |
| Modify data | db_execute_sql | INSERT/UPDATE/DELETE with transactions |
| Explore schema | db_table_schema | Faster, structured column info |
| List tables | db_list_tables | Purpose-built for table discovery |
| Check indexes | db_table_indexes | Structured index metadata |

## Security Best Practices

### Always Use readonly=true for SELECT
```json
// SAFE - prevents accidental modifications
{"sql": "SELECT * FROM users", "readonly": true}

// RISKY - allows modifications even for SELECT
{"sql": "SELECT * FROM users", "readonly": false}
```

### Limit Result Sets
```json
// SAFE - bounded memory usage
{"sql": "SELECT * FROM audit_log", "readonly": true, "max_rows": 1000}

// RISKY - could return millions of rows
{"sql": "SELECT * FROM audit_log", "readonly": true}
```

## Remember

- **Use readonly=true** for all SELECT queries to prevent accidental modifications
- **Set max_rows** to reasonable limits (50-1000) to prevent memory issues
- **Multi-statement queries** run in a single transaction (all succeed or all fail)
- **Binary data** is automatically base64-encoded in results
- **Timeouts and retries** are handled automatically with exponential backoff
