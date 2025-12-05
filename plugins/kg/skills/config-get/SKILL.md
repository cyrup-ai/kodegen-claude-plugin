---
name: config-get
description: >
  Retrieve complete KODEGEN server configuration including security settings, blocked commands, allowed directories, shell preferences, and resource limits.
  WHEN: User asks "what are the current settings", "show me the config", "what commands are blocked", "what directories are allowed", "what are my limits".
  WHEN NOT: User wants to change settings (use config_set instead).
version: 0.1.0
---

# Configuration Get

## Core Concept

`mcp__plugin_kg_kodegen__config_get` retrieves the complete server configuration as a structured object. This is a read-only operation that returns all configuration sections including security settings, shell preferences, and resource limits.

Use this tool to:
- Inspect current security restrictions (blocked commands, allowed directories)
- Check shell preferences and environment settings
- View resource limits (timeouts, file sizes, etc.)
- Debug configuration issues
- Verify settings before making changes

## Parameters

This tool takes no parameters. Simply invoke it to get the full configuration.

```json
{}
```

## Usage Examples

### Get Full Configuration
```json
{}
```

Returns the complete configuration object with all sections.

### Common Workflow: Check Before Modify
```
1. Use config_get to see current settings
2. Identify the key you want to change
3. Use config_set to modify that specific key
```

## Configuration Sections

The returned configuration typically includes:

| Section | Description |
|---------|-------------|
| `security` | Blocked commands, allowed directories, restrictions |
| `shell` | Shell preferences, environment variables |
| `resources` | Timeouts, file size limits, memory limits |
| `paths` | Working directories, temp directories |

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| View current settings | config_get | Read-only, returns full config |
| Change a setting | config_set | Modifies specific key |
| Check what's blocked | config_get | Security section has blocked commands |
| Verify allowed paths | config_get | Security section has allowed directories |

## Remember

- **No parameters needed**: Just invoke with empty object `{}`
- **Read-only operation**: Cannot modify configuration, only view it
- **Returns complete config**: All sections in one response
- **Use before config_set**: Always check current values before modifying
- **Security visibility**: Shows what commands and paths are restricted
