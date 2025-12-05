---
name: github-code-scanning-alerts
description: >
  Get code scanning (SAST) security alerts for a repository.
  WHEN: User asks about security vulnerabilities, "any code security issues", "check for vulnerabilities", "SAST results", code analysis alerts.
  WHEN NOT: Secret scanning (use github_secret_scanning_alerts), checking exposed credentials.
version: 0.1.0
---

# GitHub Code Scanning Alerts

## Core Concept

`mcp__plugin_kg_kodegen__github_code_scanning_alerts` retrieves code scanning alerts from GitHub's security analysis. These are SAST (Static Application Security Testing) results that identify potential vulnerabilities in code.

## Key Parameters

### Required
- **owner** (string): Repository owner
- **repo** (string): Repository name

### Optional
- **state** (string): Filter by state - "open", "closed", or "dismissed"
- **ref_name** (string): Filter by branch/ref
- **tool_name** (string): Filter by scanning tool (e.g., "CodeQL")
- **severity** (string): Filter by severity - "critical", "high", "medium", "low", "warning", "note", "error"

## Usage Examples

### Get all open alerts
```json
{
  "owner": "myorg",
  "repo": "my-project",
  "state": "open"
}
```

### Get critical vulnerabilities only
```json
{
  "owner": "myorg",
  "repo": "my-project",
  "state": "open",
  "severity": "critical"
}
```

### Get alerts from CodeQL on main branch
```json
{
  "owner": "myorg",
  "repo": "my-project",
  "ref_name": "main",
  "tool_name": "CodeQL"
}
```

## Output Fields

Each alert includes:
- **number**: Alert ID
- **state**: open, closed, or dismissed
- **severity**: Severity level
- **rule_id**: Security rule identifier
- **rule_description**: What the rule checks
- **tool_name**: Scanning tool that found it
- **created_at**: When discovered
- **html_url**: GitHub URL to view alert

## Severity Levels

- **critical**: Immediate action required
- **high**: Important security issue
- **medium**: Moderate concern
- **low**: Minor issue
- **warning/note/error**: Tool-specific classifications

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Code vulnerabilities | github_code_scanning_alerts | SAST results |
| Exposed secrets | github_secret_scanning_alerts | Credential leaks |
| Security overview | Both tools | Complete picture |

## Remember

- Requires GitHub Advanced Security or public repo with code scanning enabled
- Code scanning must be configured in repo settings
- CodeQL is GitHub's primary scanning tool
- Filter by severity to prioritize fixes
- Dismissed alerts stay in history unless deleted
