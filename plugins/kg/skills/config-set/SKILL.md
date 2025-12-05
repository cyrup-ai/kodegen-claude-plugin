---
name: config-set
description: >
  Set a specific KODEGEN configuration value by key. Supports string, number, boolean, and array values.
  WHEN: User wants to "change a setting", "update config", "set timeout", "add allowed directory", "modify preferences".
  WHEN NOT: User wants to view current settings (use config_get instead).
version: 0.1.0
---

# Configuration Set

## Core Concept

`mcp__plugin_kg_kodegen__config_set` modifies a specific configuration value by key. This is a write operation that persists changes to the server configuration.

**SECURITY WARNING**: This tool should be used in a separate chat session from file operations and command execution to prevent security bypass through configuration manipulation.

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `key` | string | Yes | Configuration key to update (dot notation supported) |
| `value` | ConfigValue | Yes | New value (string, number, boolean, or string array) |

### ConfigValue Types

The `value` parameter accepts multiple types:

```typescript
type ConfigValue = string | number | boolean | string[];
```

## Usage Examples

### Set a String Value
```json
{
  "key": "shell.default",
  "value": "/bin/zsh"
}
```

### Set a Numeric Value
```json
{
  "key": "resources.timeout_seconds",
  "value": 120
}
```

### Set a Boolean Value
```json
{
  "key": "security.allow_network",
  "value": true
}
```

### Set an Array Value
```json
{
  "key": "security.allowed_directories",
  "value": ["/home/user/projects", "/tmp", "/var/data"]
}
```

### Add to Blocked Commands
```json
{
  "key": "security.blocked_commands",
  "value": ["rm -rf", "sudo", "chmod 777"]
}
```

## Common Configuration Keys

| Key | Type | Description |
|-----|------|-------------|
| `security.blocked_commands` | string[] | Commands that cannot be executed |
| `security.allowed_directories` | string[] | Directories that can be accessed |
| `resources.timeout_seconds` | number | Default command timeout |
| `resources.max_file_size` | number | Maximum file size in bytes |
| `shell.default` | string | Default shell to use |

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| View current settings | config_get | Read-only inspection |
| Change a setting | config_set | Modifies specific key |
| Change multiple settings | config_set (multiple calls) | One key per call |
| Reset to defaults | config_set | Set key to default value |

## Remember

- **SECURITY WARNING**: Use in separate chat from file/command operations
- **One key at a time**: Each call modifies one configuration key
- **Check first**: Use config_get to verify current value before changing
- **Value types matter**: Use correct type (string, number, boolean, array)
- **Persistent changes**: Modifications are saved and persist across sessions
- **Dot notation**: Use dots to access nested keys (e.g., `security.blocked_commands`)
