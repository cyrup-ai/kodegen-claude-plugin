---
title: "Plugins reference - Claude Code Docs"
source_url: "https://code.claude.com/docs/en/plugins-reference"
word_count: 1625
reading_time: "9 min read"
date_converted: "2025-12-01T17:34:57.908Z"
---
# Plugins reference - Claude Code Docs

This reference provides complete technical specifications for the Claude Code plugin system, including component schemas, CLI commands, and development tools.

Plugin components reference
---------------------------

This section documents the five types of components that plugins can provide.

### Commands

Plugins add custom slash commands that integrate seamlessly with Claude Code’s command system.**Location**: `commands/` directory in plugin root**File format**: Markdown files with frontmatter For complete details on plugin command structure, invocation patterns, and features, see [Plugin commands](https://code.claude.com/docs/en/slash-commands#plugin-commands).

### Agents

Plugins can provide specialized subagents for specific tasks that Claude can invoke automatically when appropriate.**Location**: `agents/` directory in plugin root**File format**: Markdown files describing agent capabilities**Agent structure**:

```
---
description: What this agent specializes in
capabilities: ["task1", "task2", "task3"]
---

# Agent Name

Detailed description of the agent's role, expertise, and when Claude should invoke it.

## Capabilities
- Specific task the agent excels at
- Another specialized capability
- When to use this agent vs others

## Context and examples
Provide examples of when this agent should be used and what kinds of problems it solves.
```

**Integration points**:

*   Agents appear in the `/agents` interface
*   Claude can invoke agents automatically based on task context
*   Agents can be invoked manually by users
*   Plugin agents work alongside built-in Claude agents

### Skills

Plugins can provide Agent Skills that extend Claude’s capabilities. Skills are model-invoked—Claude autonomously decides when to use them based on the task context.**Location**: `skills/` directory in plugin root**File format**: Directories containing `SKILL.md` files with frontmatter**Skill structure**:

```
skills/
├── pdf-processor/
│   ├── SKILL.md
│   ├── reference.md (optional)
│   └── scripts/ (optional)
└── code-reviewer/
    └── SKILL.md
```

**Integration behavior**:

*   Plugin Skills are automatically discovered when the plugin is installed
*   Claude autonomously invokes Skills based on matching task context
*   Skills can include supporting files alongside SKILL.md

For SKILL.md format and complete Skill authoring guidance, see:

*   [Use Skills in Claude Code](https://code.claude.com/docs/en/skills)
*   [Agent Skills overview](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview#skill-structure)

### Hooks

Plugins can provide event handlers that respond to Claude Code events automatically.**Location**: `hooks/hooks.json` in plugin root, or inline in plugin.json**Format**: JSON configuration with event matchers and actions**Hook configuration**:

```
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "${CLAUDE_PLUGIN_ROOT}/scripts/format-code.sh"
          }
        ]
      }
    ]
  }
}
```

**Available events**:

*   `PreToolUse`: Before Claude uses any tool
*   `PermissionRequest`: When a permission dialog is shown
*   `PostToolUse`: After Claude uses any tool
*   `UserPromptSubmit`: When user submits a prompt
*   `Notification`: When Claude Code sends notifications
*   `Stop`: When Claude attempts to stop
*   `SubagentStop`: When a subagent attempts to stop
*   `SessionStart`: At the beginning of sessions
*   `SessionEnd`: At the end of sessions
*   `PreCompact`: Before conversation history is compacted

**Hook types**:

*   `command`: Execute shell commands or scripts
*   `validation`: Validate file contents or project state
*   `notification`: Send alerts or status updates

### MCP servers

Plugins can bundle Model Context Protocol (MCP) servers to connect Claude Code with external tools and services.**Location**: `.mcp.json` in plugin root, or inline in plugin.json**Format**: Standard MCP server configuration**MCP server configuration**:

```
{
  "mcpServers": {
    "plugin-database": {
      "command": "${CLAUDE_PLUGIN_ROOT}/servers/db-server",
      "args": ["--config", "${CLAUDE_PLUGIN_ROOT}/config.json"],
      "env": {
        "DB_PATH": "${CLAUDE_PLUGIN_ROOT}/data"
      }
    },
    "plugin-api-client": {
      "command": "npx",
      "args": ["@company/mcp-server", "--plugin-mode"],
      "cwd": "${CLAUDE_PLUGIN_ROOT}"
    }
  }
}
```

**Integration behavior**:

*   Plugin MCP servers start automatically when the plugin is enabled
*   Servers appear as standard MCP tools in Claude’s toolkit
*   Server capabilities integrate seamlessly with Claude’s existing tools
*   Plugin servers can be configured independently of user MCP servers

* * *

Plugin manifest schema
----------------------

The `plugin.json` file defines your plugin’s metadata and configuration. This section documents all supported fields and options.

### Complete schema

```
{
  "name": "plugin-name",
  "version": "1.2.0",
  "description": "Brief plugin description",
  "author": {
    "name": "Author Name",
    "email": "[email protected]",
    "url": "https://github.com/author"
  },
  "homepage": "https://docs.example.com/plugin",
  "repository": "https://github.com/author/plugin",
  "license": "MIT",
  "keywords": ["keyword1", "keyword2"],
  "commands": ["./custom/commands/special.md"],
  "agents": "./custom/agents/",
  "hooks": "./config/hooks.json",
  "mcpServers": "./mcp-config.json"
}
```

### Required fields

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `name` | string | Unique identifier (kebab-case, no spaces) | `"deployment-tools"` |

### Metadata fields

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `version` | string | Semantic version | `"2.1.0"` |
| `description` | string | Brief explanation of plugin purpose | `"Deployment automation tools"` |
| `author` | object | Author information | `{"name": "Dev Team", "email": "[email protected]"}` |
| `homepage` | string | Documentation URL | `"https://docs.example.com"` |
| `repository` | string | Source code URL | `"https://github.com/user/plugin"` |
| `license` | string | License identifier | `"MIT"`, `"Apache-2.0"` |
| `keywords` | array | Discovery tags | `["deployment", "ci-cd"]` |

### Component path fields

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `commands` | string|array | Additional command files/directories | `"./custom/cmd.md"` or `["./cmd1.md"]` |
| `agents` | string|array | Additional agent files | `"./custom/agents/"` |
| `hooks` | string|object | Hook config path or inline config | `"./hooks.json"` |
| `mcpServers` | string|object | MCP config path or inline config | `"./mcp.json"` |

### Path behavior rules

**Important**: Custom paths supplement default directories - they don’t replace them.

*   If `commands/` exists, it’s loaded in addition to custom command paths
*   All paths must be relative to plugin root and start with `./`
*   Commands from custom paths use the same naming and namespacing rules
*   Multiple paths can be specified as arrays for flexibility

**Path examples**:

```
{
  "commands": [
    "./specialized/deploy.md",
    "./utilities/batch-process.md"
  ],
  "agents": [
    "./custom-agents/reviewer.md",
    "./custom-agents/tester.md"
  ]
}
```

### Environment variables

**`${CLAUDE_PLUGIN_ROOT}`**: Contains the absolute path to your plugin directory. Use this in hooks, MCP servers, and scripts to ensure correct paths regardless of installation location.

```
{
  "hooks": {
    "PostToolUse": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "${CLAUDE_PLUGIN_ROOT}/scripts/process.sh"
          }
        ]
      }
    ]
  }
}
```

* * *

Plugin directory structure
--------------------------

### Standard plugin layout

A complete plugin follows this structure:

```
enterprise-plugin/
├── .claude-plugin/           # Metadata directory
│   └── plugin.json          # Required: plugin manifest
├── commands/                 # Default command location
│   ├── status.md
│   └──  logs.md
├── agents/                   # Default agent location
│   ├── security-reviewer.md
│   ├── performance-tester.md
│   └── compliance-checker.md
├── skills/                   # Agent Skills
│   ├── code-reviewer/
│   │   └── SKILL.md
│   └── pdf-processor/
│       ├── SKILL.md
│       └── scripts/
├── hooks/                    # Hook configurations
│   ├── hooks.json           # Main hook config
│   └── security-hooks.json  # Additional hooks
├── .mcp.json                # MCP server definitions
├── scripts/                 # Hook and utility scripts
│   ├── security-scan.sh
│   ├── format-code.py
│   └── deploy.js
├── LICENSE                  # License file
└── CHANGELOG.md             # Version history
```

### File locations reference

| Component | Default Location | Purpose |
| --- | --- | --- |
| **Manifest** | `.claude-plugin/plugin.json` | Required metadata file |
| **Commands** | `commands/` | Slash command markdown files |
| **Agents** | `agents/` | Subagent markdown files |
| **Skills** | `skills/` | Agent Skills with SKILL.md files |
| **Hooks** | `hooks/hooks.json` | Hook configuration |
| **MCP servers** | `.mcp.json` | MCP server definitions |

* * *

Debugging and development tools
-------------------------------

### Debugging commands

Use `claude --debug` to see plugin loading details:

```
claude --debug
```

This shows:

*   Which plugins are being loaded
*   Any errors in plugin manifests
*   Command, agent, and hook registration
*   MCP server initialization

### Common issues

| Issue | Cause | Solution |
| --- | --- | --- |
| Plugin not loading | Invalid `plugin.json` | Validate JSON syntax |
| Commands not appearing | Wrong directory structure | Ensure `commands/` at root, not in `.claude-plugin/` |
| Hooks not firing | Script not executable | Run `chmod +x script.sh` |
| MCP server fails | Missing `${CLAUDE_PLUGIN_ROOT}` | Use variable for all plugin paths |
| Path errors | Absolute paths used | All paths must be relative and start with `./` |

* * *

Distribution and versioning reference
-------------------------------------

### Version management

Follow semantic versioning for plugin releases:

```
## See also

- [Plugins](/en/plugins) - Tutorials and practical usage
- [Plugin marketplaces](/en/plugin-marketplaces) - Creating and managing marketplaces
- [Slash commands](/en/slash-commands) - Command development details
- [Subagents](/en/sub-agents) - Agent configuration and capabilities
- [Agent Skills](/en/skills) - Extend Claude's capabilities
- [Hooks](/en/hooks) - Event handling and automation
- [MCP](/en/mcp) - External tool integration
- [Settings](/en/settings) - Configuration options for plugins
```

## Links

- [[email protected]](https://code.claude.com/cdn-cgi/l/email-protection)
- [Administration](https://code.claude.com/docs/en/setup)
- [Agent Skills overview](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview#skill-structure)
- [Agents](https://code.claude.com/docs/en/plugins-reference#agents)
- [Anthropic](https://www.anthropic.com/company)
- [Availability](https://www.anthropic.com/supported-countries)
- [Build with Claude Code](https://code.claude.com/docs/en/sub-agents)
- [Careers](https://www.anthropic.com/careers)
- [Checkpointing](https://code.claude.com/docs/en/checkpointing)
- [Claude Code Docs home page](https://code.claude.com/docs)
- [Claude Code on the Web](https://claude.ai/code)
- [Claude Developer Platform](https://platform.claude.com/)
- [Commands](https://code.claude.com/docs/en/plugins-reference#commands)
- [Commercial terms](https://www.anthropic.com/legal/commercial-terms)
- [Common issues](https://code.claude.com/docs/en/plugins-reference#common-issues)
- [Complete schema](https://code.claude.com/docs/en/plugins-reference#complete-schema)
- [Component path fields](https://code.claude.com/docs/en/plugins-reference#component-path-fields)
- [Configuration](https://code.claude.com/docs/en/settings)
- [Consumer terms](https://www.anthropic.com/legal/consumer-terms)
- [Courses](https://www.anthropic.com/learn)
- [Customer stories](https://www.claude.com/customers)
- [Debugging and development tools](https://code.claude.com/docs/en/plugins-reference#debugging-and-development-tools)
- [Debugging commands](https://code.claude.com/docs/en/plugins-reference#debugging-commands)
- [Deployment](https://code.claude.com/docs/en/third-party-integrations)
- [Disclosure policy](https://www.anthropic.com/responsible-disclosure-policy)
- [Distribution and versioning reference](https://code.claude.com/docs/en/plugins-reference#distribution-and-versioning-reference)
- [Economic Futures](https://www.anthropic.com/economic-futures)
- [Engineering blog](https://www.anthropic.com/engineering)
- [Environment variables](https://code.claude.com/docs/en/plugins-reference#environment-variables)
- [Events](https://www.anthropic.com/events)
- [File locations reference](https://code.claude.com/docs/en/plugins-reference#file-locations-reference)
- [Getting started](https://code.claude.com/docs/en/overview)
- [Hooks](https://code.claude.com/docs/en/plugins-reference#hooks)
- [Hooks reference](https://code.claude.com/docs/en/hooks)
- [Interactive mode](https://code.claude.com/docs/en/interactive-mode)
- [linkedin](https://www.linkedin.com/company/anthropicresearch)
- [MCP connectors](https://claude.com/partners/mcp)
- [MCP servers](https://code.claude.com/docs/en/plugins-reference#mcp-servers)
- [Metadata fields](https://code.claude.com/docs/en/plugins-reference#metadata-fields)
- [News](https://www.anthropic.com/news)
- [Path behavior rules](https://code.claude.com/docs/en/plugins-reference#path-behavior-rules)
- [Plugin commands](https://code.claude.com/docs/en/slash-commands#plugin-commands)
- [Plugin components reference](https://code.claude.com/docs/en/plugins-reference#plugin-components-reference)
- [Plugin directory structure](https://code.claude.com/docs/en/plugins-reference#plugin-directory-structure)
- [Plugin manifest schema](https://code.claude.com/docs/en/plugins-reference#plugin-manifest-schema)
- [Plugin marketplaces](https://code.claude.com/docs/en/plugin-marketplaces)
- [Plugins](https://code.claude.com/docs/en/plugins)
- [Plugins reference](https://code.claude.com/docs/en/plugins-reference)
- [Powered by Claude](https://claude.com/partners/powered-by-claude)
- [Privacy policy](https://www.anthropic.com/legal/privacy)
- [Reference](https://code.claude.com/docs/en/cli-reference)
- [Required fields](https://code.claude.com/docs/en/plugins-reference#required-fields)
- [Research](https://www.anthropic.com/research)
- [Resources](https://code.claude.com/docs/en/legal-and-compliance)
- [Service partners](https://claude.com/partners/services)
- [Skills](https://code.claude.com/docs/en/plugins-reference#skills)
- [Skip to main content](https://code.claude.com/docs/en/plugins-reference#content-area)
- [Slash commands](https://code.claude.com/docs/en/slash-commands)
- [Standard plugin layout](https://code.claude.com/docs/en/plugins-reference#standard-plugin-layout)
- [Startups program](https://claude.com/programs/startups)
- [Status](https://status.anthropic.com/)
- [Support center](https://support.claude.com/)
- [Transparency](https://www.anthropic.com/transparency)
- [Trust center](https://trust.anthropic.com/)
- [Usage policy](https://www.anthropic.com/legal/aup)
- [Use Skills in Claude Code](https://code.claude.com/docs/en/skills)
- [Version management](https://code.claude.com/docs/en/plugins-reference#version-management)
- [x](https://x.com/AnthropicAI)
