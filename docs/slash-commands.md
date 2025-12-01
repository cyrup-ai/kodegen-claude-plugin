---
title: "Slash commands - Claude Code Docs"
source_url: "https://code.claude.com/docs/en/slash-commands"
word_count: 2675
reading_time: "14 min read"
date_converted: "2025-12-01T17:36:32.481Z"
---
# Slash commands - Claude Code Docs

Built-in slash commands
-----------------------

| Command | Purpose |
| --- | --- |
| `/add-dir` | Add additional working directories |
| `/agents` | Manage custom AI subagents for specialized tasks |
| `/bashes` | List and manage background tasks |
| `/bug` | Report bugs (sends conversation to Anthropic) |
| `/clear` | Clear conversation history |
| `/compact [instructions]` | Compact conversation with optional focus instructions |
| `/config` | Open the Settings interface (Config tab) |
| `/context` | Visualize current context usage as a colored grid |
| `/cost` | Show token usage statistics (see [cost tracking guide](https://code.claude.com/docs/en/costs#using-the-cost-command) for subscription-specific details) |
| `/doctor` | Checks the health of your Claude Code installation |
| `/exit` | Exit the REPL |
| `/export [filename]` | Export the current conversation to a file or clipboard |
| `/help` | Get usage help |
| `/hooks` | Manage hook configurations for tool events |
| `/ide` | Manage IDE integrations and show status |
| `/init` | Initialize project with CLAUDE.md guide |
| `/install-github-app` | Set up Claude GitHub Actions for a repository |
| `/login` | Switch Anthropic accounts |
| `/logout` | Sign out from your Anthropic account |
| `/mcp` | Manage MCP server connections and OAuth authentication |
| `/memory` | Edit CLAUDE.md memory files |
| `/model` | Select or change the AI model |
| `/output-style [style]` | Set the output style directly or from a selection menu |
| `/permissions` | View or update [permissions](https://code.claude.com/docs/en/iam#configuring-permissions) |
| `/plugin` | Manage Claude Code plugins |
| `/pr-comments` | View pull request comments |
| `/privacy-settings` | View and update your privacy settings |
| `/release-notes` | View release notes |
| `/resume` | Resume a conversation |
| `/review` | Request code review |
| `/rewind` | Rewind the conversation and/or code |
| `/sandbox` | Enable sandboxed bash tool with filesystem and network isolation for safer, more autonomous execution |
| `/security-review` | Complete a security review of pending changes on the current branch |
| `/status` | Open the Settings interface (Status tab) showing version, model, account, and connectivity |
| `/statusline` | Set up Claude Code’s status line UI |
| `/terminal-setup` | Install Shift+Enter key binding for newlines (iTerm2 and VSCode only) |
| `/todos` | List current todo items |
| `/usage` | Show plan usage limits and rate limit status (subscription plans only) |
| `/vim` | Enter vim mode for alternating insert and command modes |

Custom slash commands
---------------------

Custom slash commands allow you to define frequently-used prompts as Markdown files that Claude Code can execute. Commands are organized by scope (project-specific or personal) and support namespacing through directory structures.

### Syntax

```
/<command-name> [arguments]
```

#### Parameters

| Parameter | Description |
| --- | --- |
| `<command-name>` | Name derived from the Markdown filename (without `.md` extension) |
| `[arguments]` | Optional arguments passed to the command |

### Command types

#### Project commands

Commands stored in your repository and shared with your team. When listed in `/help`, these commands show “(project)” after their description.**Location**: `.claude/commands/`In the following example, we create the `/optimize` command:

```
# Create a project command
mkdir -p .claude/commands
echo "Analyze this code for performance issues and suggest optimizations:" > .claude/commands/optimize.md
```

#### Personal commands

Commands available across all your projects. When listed in `/help`, these commands show “(user)” after their description.**Location**: `~/.claude/commands/`In the following example, we create the `/security-review` command:

```
# Create a personal command
mkdir -p ~/.claude/commands
echo "Review this code for security vulnerabilities:" > ~/.claude/commands/security-review.md
```

### Features

#### Namespacing

Organize commands in subdirectories. The subdirectories are used for organization and appear in the command description, but they do not affect the command name itself. The description will show whether the command comes from the project directory (`.claude/commands`) or the user-level directory (`~/.claude/commands`), along with the subdirectory name.Conflicts between user and project level commands are not supported. Otherwise, multiple commands with the same base file name can coexist.For example, a file at `.claude/commands/frontend/component.md` creates the command `/component` with description showing “(project:frontend)”. Meanwhile, a file at `~/.claude/commands/component.md` creates the command `/component` with description showing “(user)”.

#### Arguments

Pass dynamic values to commands using argument placeholders:

##### All arguments with `$ARGUMENTS`

The `$ARGUMENTS` placeholder captures all arguments passed to the command:

```
# Command definition
echo 'Fix issue #$ARGUMENTS following our coding standards' > .claude/commands/fix-issue.md

# Usage
> /fix-issue 123 high-priority
# $ARGUMENTS becomes: "123 high-priority"
```

##### Individual arguments with `$1`, `$2`, etc.

Access specific arguments individually using positional parameters (similar to shell scripts):

```
# Command definition  
echo 'Review PR #$1 with priority $2 and assign to $3' > .claude/commands/review-pr.md

# Usage
> /review-pr 456 high alice
# $1 becomes "456", $2 becomes "high", $3 becomes "alice"
```

Use positional arguments when you need to:

*   Access arguments individually in different parts of your command
*   Provide defaults for missing arguments
*   Build more structured commands with specific parameter roles

#### Bash command execution

Execute bash commands before the slash command runs using the `!` prefix. The output is included in the command context. You _must_ include `allowed-tools` with the `Bash` tool, but you can choose the specific bash commands to allow.For example:

```
---
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git commit:*)
description: Create a git commit
---

## Context

- Current git status: !`git status`
- Current git diff (staged and unstaged changes): !`git diff HEAD`
- Current branch: !`git branch --show-current`
- Recent commits: !`git log --oneline -10`

## Your task

Based on the above changes, create a single git commit.
```

#### File references

Include file contents in commands using the `@` prefix to [reference files](https://code.claude.com/docs/en/common-workflows#reference-files-and-directories).For example:

```
# Reference a specific file

Review the implementation in @src/utils/helpers.js

# Reference multiple files

Compare @src/old-version.js with @src/new-version.js
```

#### Thinking mode

Slash commands can trigger extended thinking by including [extended thinking keywords](https://code.claude.com/docs/en/common-workflows#use-extended-thinking).

### Frontmatter

Command files support frontmatter, useful for specifying metadata about the command:

| Frontmatter | Purpose | Default |
| --- | --- | --- |
| `allowed-tools` | List of tools the command can use | Inherits from the conversation |
| `argument-hint` | The arguments expected for the slash command. Example: `argument-hint: add [tagId] | remove [tagId] | list`. This hint is shown to the user when auto-completing the slash command. | None |
| `description` | Brief description of the command | Uses the first line from the prompt |
| `model` | Specific model string (see [Models overview](https://docs.claude.com/en/docs/about-claude/models/overview)) | Inherits from the conversation |
| `disable-model-invocation` | Whether to prevent `SlashCommand` tool from calling this command | false |

For example:

```
---
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git commit:*)
argument-hint: [message]
description: Create a git commit
model: claude-3-5-haiku-20241022
---

Create a git commit with message: $ARGUMENTS
```

Example using positional arguments:

```
---
argument-hint: [pr-number] [priority] [assignee]
description: Review pull request
---

Review PR #$1 with priority $2 and assign to $3.
Focus on security, performance, and code style.
```

Plugin commands
---------------

[Plugins](https://code.claude.com/docs/en/plugins) can provide custom slash commands that integrate seamlessly with Claude Code. Plugin commands work exactly like user-defined commands but are distributed through [plugin marketplaces](https://code.claude.com/docs/en/plugin-marketplaces).

### How plugin commands work

Plugin commands are:

*   **Namespaced**: Commands can use the format `/plugin-name:command-name` to avoid conflicts (plugin prefix is optional unless there are name collisions)
*   **Automatically available**: Once a plugin is installed and enabled, its commands appear in `/help`
*   **Fully integrated**: Support all command features (arguments, frontmatter, bash execution, file references)

### Plugin command structure

**Location**: `commands/` directory in plugin root**File format**: Markdown files with frontmatter**Basic command structure**:

```
---
description: Brief description of what the command does
---

# Command Name

Detailed instructions for Claude on how to execute this command.
Include specific guidance on parameters, expected outcomes, and any special considerations.
```

**Advanced command features**:

*   **Arguments**: Use placeholders like `{arg1}` in command descriptions
*   **Subdirectories**: Organize commands in subdirectories for namespacing
*   **Bash integration**: Commands can execute shell scripts and programs
*   **File references**: Commands can reference and modify project files

### Invocation patterns

Direct command (when no conflicts)

```
/command-name
```

Plugin-prefixed (when needed for disambiguation)

```
/plugin-name:command-name
```

With arguments (if command supports them)

```
/command-name arg1 arg2
```

MCP servers can expose prompts as slash commands that become available in Claude Code. These commands are dynamically discovered from connected MCP servers.

### Command format

MCP commands follow the pattern:

```
/mcp__<server-name>__<prompt-name> [arguments]
```

### Features

#### Dynamic discovery

MCP commands are automatically available when:

*   An MCP server is connected and active
*   The server exposes prompts through the MCP protocol
*   The prompts are successfully retrieved during connection

#### Arguments

MCP prompts can accept arguments defined by the server:

```
# Without arguments
> /mcp__github__list_prs

# With arguments
> /mcp__github__pr_review 456
> /mcp__jira__create_issue "Bug title" high
```

#### Naming conventions

*   Server and prompt names are normalized
*   Spaces and special characters become underscores
*   Names are lowercased for consistency

### Managing MCP connections

Use the `/mcp` command to:

*   View all configured MCP servers
*   Check connection status
*   Authenticate with OAuth-enabled servers
*   Clear authentication tokens
*   View available tools and prompts from each server

### MCP permissions and wildcards

When configuring [permissions for MCP tools](https://code.claude.com/docs/en/iam#tool-specific-permission-rules), note that **wildcards are not supported**:

*   ✅ **Correct**: `mcp__github` (approves ALL tools from the github server)
*   ✅ **Correct**: `mcp__github__get_issue` (approves specific tool)
*   ❌ **Incorrect**: `mcp__github__*` (wildcards not supported)

To approve all tools from an MCP server, use just the server name: `mcp__servername`. To approve specific tools only, list each tool individually.

`SlashCommand` tool
-------------------

The `SlashCommand` tool allows Claude to execute [custom slash commands](https://code.claude.com/docs/en/slash-commands#custom-slash-commands) programmatically during a conversation. This gives Claude the ability to invoke custom commands on your behalf when appropriate.To encourage Claude to trigger `SlashCommand` tool, your instructions (prompts, CLAUDE.md, etc.) generally need to reference the command by name with its slash.Example:

```
> Run /write-unit-test when you are about to start writing tests.
```

This tool puts each available custom slash command’s metadata into context up to the character budget limit. You can use `/context` to monitor token usage and follow the operations below to manage context.

### `SlashCommand` tool supported commands

`SlashCommand` tool only supports custom slash commands that:

*   Are user-defined. Built-in commands like `/compact` and `/init` are _not_ supported.
*   Have the `description` frontmatter field populated. We use the `description` in the context.

For Claude Code versions >= 1.0.124, you can see which custom slash commands `SlashCommand` tool can invoke by running `claude --debug` and triggering a query.

### Disable `SlashCommand` tool

To prevent Claude from executing any slash commands via the tool:

```
/permissions
# Add to deny rules: SlashCommand
```

This will also remove SlashCommand tool (and the slash command descriptions) from context.

### Disable specific commands only

To prevent a specific slash command from becoming available, add `disable-model-invocation: true` to the slash command’s frontmatter.This will also remove the command’s metadata from context.

### `SlashCommand` permission rules

The permission rules support:

*   **Exact match**: `SlashCommand:/commit` (allows only `/commit` with no arguments)
*   **Prefix match**: `SlashCommand:/review-pr:*` (allows `/review-pr` with any arguments)

### Character budget limit

The `SlashCommand` tool includes a character budget to limit the size of command descriptions shown to Claude. This prevents token overflow when many commands are available.The budget includes each custom slash command’s name, args, and description.

*   **Default limit**: 15,000 characters
*   **Custom limit**: Set via `SLASH_COMMAND_TOOL_CHAR_BUDGET` environment variable

When the character budget is exceeded, Claude will see only a subset of the available commands. In `/context`, a warning will show with “M of N commands”.

Skills vs slash commands
------------------------

**Slash commands** and **Agent Skills** serve different purposes in Claude Code:

### Use slash commands for

**Quick, frequently-used prompts**:

*   Simple prompt snippets you use often
*   Quick reminders or templates
*   Frequently-used instructions that fit in one file

**Examples**:

*   `/review` → “Review this code for bugs and suggest improvements”
*   `/explain` → “Explain this code in simple terms”
*   `/optimize` → “Analyze this code for performance issues”

### Use Skills for

**Comprehensive capabilities with structure**:

*   Complex workflows with multiple steps
*   Capabilities requiring scripts or utilities
*   Knowledge organized across multiple files
*   Team workflows you want to standardize

**Examples**:

*   PDF processing Skill with form-filling scripts and validation
*   Data analysis Skill with reference docs for different data types
*   Documentation Skill with style guides and templates

### Key differences

| Aspect | Slash Commands | Agent Skills |
| --- | --- | --- |
| **Complexity** | Simple prompts | Complex capabilities |
| **Structure** | Single .md file | Directory with SKILL.md + resources |
| **Discovery** | Explicit invocation (`/command`) | Automatic (based on context) |
| **Files** | One file only | Multiple files, scripts, templates |
| **Scope** | Project or personal | Project or personal |
| **Sharing** | Via git | Via git |

### Example comparison

**As a slash command**:

```
# .claude/commands/review.md
Review this code for:
- Security vulnerabilities
- Performance issues
- Code style violations
```

Usage: `/review` (manual invocation)**As a Skill**:

```
.claude/skills/code-review/
├── SKILL.md (overview and workflows)
├── SECURITY.md (security checklist)
├── PERFORMANCE.md (performance patterns)
├── STYLE.md (style guide reference)
└── scripts/
    └── run-linters.sh
```

Usage: “Can you review this code?” (automatic discovery)The Skill provides richer context, validation scripts, and organized reference material.

### When to use each

**Use slash commands**:

*   You invoke the same prompt repeatedly
*   The prompt fits in a single file
*   You want explicit control over when it runs

**Use Skills**:

*   Claude should discover the capability automatically
*   Multiple files or scripts are needed
*   Complex workflows with validation steps
*   Team needs standardized, detailed guidance

Both slash commands and Skills can coexist. Use the approach that fits your needs.Learn more about [Agent Skills](https://code.claude.com/docs/en/skills).

See also
--------

*   [Plugins](https://code.claude.com/docs/en/plugins) - Extend Claude Code with custom commands through plugins
*   [Identity and Access Management](https://code.claude.com/docs/en/iam) - Complete guide to permissions, including MCP tool permissions
*   [Interactive mode](https://code.claude.com/docs/en/interactive-mode) - Shortcuts, input modes, and interactive features
*   [CLI reference](https://code.claude.com/docs/en/cli-reference) - Command-line flags and options
*   [Settings](https://code.claude.com/docs/en/settings) - Configuration options
*   [Memory management](https://code.claude.com/docs/en/memory) - Managing Claude’s memory across sessions

## Links

- [Administration](https://code.claude.com/docs/en/setup)
- [Agent Skills](https://code.claude.com/docs/en/skills)
- [Anthropic](https://www.anthropic.com/company)
- [Arguments](https://code.claude.com/docs/en/slash-commands#arguments-2)
- [Availability](https://www.anthropic.com/supported-countries)
- [Bash command execution](https://code.claude.com/docs/en/slash-commands#bash-command-execution)
- [Build with Claude Code](https://code.claude.com/docs/en/sub-agents)
- [Built-in slash commands](https://code.claude.com/docs/en/slash-commands#built-in-slash-commands)
- [Careers](https://www.anthropic.com/careers)
- [Character budget limit](https://code.claude.com/docs/en/slash-commands#character-budget-limit)
- [Checkpointing](https://code.claude.com/docs/en/checkpointing)
- [Claude Code Docs home page](https://code.claude.com/docs)
- [Claude Code on the Web](https://claude.ai/code)
- [Claude Developer Platform](https://platform.claude.com/)
- [Command format](https://code.claude.com/docs/en/slash-commands#command-format)
- [Command types](https://code.claude.com/docs/en/slash-commands#command-types)
- [Commercial terms](https://www.anthropic.com/legal/commercial-terms)
- [Configuration](https://code.claude.com/docs/en/settings)
- [Consumer terms](https://www.anthropic.com/legal/consumer-terms)
- [cost tracking guide](https://code.claude.com/docs/en/costs#using-the-cost-command)
- [Courses](https://www.anthropic.com/learn)
- [Custom slash commands](https://code.claude.com/docs/en/slash-commands#custom-slash-commands)
- [Customer stories](https://www.claude.com/customers)
- [Deployment](https://code.claude.com/docs/en/third-party-integrations)
- [Disable SlashCommand tool](https://code.claude.com/docs/en/slash-commands#disable-slashcommand-tool)
- [Disable specific commands only](https://code.claude.com/docs/en/slash-commands#disable-specific-commands-only)
- [Disclosure policy](https://www.anthropic.com/responsible-disclosure-policy)
- [Dynamic discovery](https://code.claude.com/docs/en/slash-commands#dynamic-discovery)
- [Economic Futures](https://www.anthropic.com/economic-futures)
- [Engineering blog](https://www.anthropic.com/engineering)
- [Events](https://www.anthropic.com/events)
- [Example comparison](https://code.claude.com/docs/en/slash-commands#example-comparison)
- [extended thinking keywords](https://code.claude.com/docs/en/common-workflows#use-extended-thinking)
- [Features](https://code.claude.com/docs/en/slash-commands#features-2)
- [File references](https://code.claude.com/docs/en/slash-commands#file-references)
- [Frontmatter](https://code.claude.com/docs/en/slash-commands#frontmatter)
- [Getting started](https://code.claude.com/docs/en/overview)
- [Hooks reference](https://code.claude.com/docs/en/hooks)
- [How plugin commands work](https://code.claude.com/docs/en/slash-commands#how-plugin-commands-work)
- [Identity and Access Management](https://code.claude.com/docs/en/iam)
- [Interactive mode](https://code.claude.com/docs/en/interactive-mode)
- [Invocation patterns](https://code.claude.com/docs/en/slash-commands#invocation-patterns)
- [Key differences](https://code.claude.com/docs/en/slash-commands#key-differences)
- [linkedin](https://www.linkedin.com/company/anthropicresearch)
- [Managing MCP connections](https://code.claude.com/docs/en/slash-commands#managing-mcp-connections)
- [MCP connectors](https://claude.com/partners/mcp)
- [MCP permissions and wildcards](https://code.claude.com/docs/en/slash-commands#mcp-permissions-and-wildcards)
- [MCP slash commands](https://code.claude.com/docs/en/slash-commands#mcp-slash-commands)
- [Memory management](https://code.claude.com/docs/en/memory)
- [Models overview](https://docs.claude.com/en/docs/about-claude/models/overview)
- [Namespacing](https://code.claude.com/docs/en/slash-commands#namespacing)
- [Naming conventions](https://code.claude.com/docs/en/slash-commands#naming-conventions)
- [News](https://www.anthropic.com/news)
- [Parameters](https://code.claude.com/docs/en/slash-commands#parameters)
- [permissions](https://code.claude.com/docs/en/iam#configuring-permissions)
- [permissions for MCP tools](https://code.claude.com/docs/en/iam#tool-specific-permission-rules)
- [Personal commands](https://code.claude.com/docs/en/slash-commands#personal-commands)
- [Plugin command structure](https://code.claude.com/docs/en/slash-commands#plugin-command-structure)
- [Plugin commands](https://code.claude.com/docs/en/slash-commands#plugin-commands)
- [plugin marketplaces](https://code.claude.com/docs/en/plugin-marketplaces)
- [Plugins](https://code.claude.com/docs/en/plugins)
- [Plugins reference](https://code.claude.com/docs/en/plugins-reference)
- [Powered by Claude](https://claude.com/partners/powered-by-claude)
- [Privacy policy](https://www.anthropic.com/legal/privacy)
- [Project commands](https://code.claude.com/docs/en/slash-commands#project-commands)
- [Reference](https://code.claude.com/docs/en/cli-reference)
- [reference files](https://code.claude.com/docs/en/common-workflows#reference-files-and-directories)
- [Research](https://www.anthropic.com/research)
- [Resources](https://code.claude.com/docs/en/legal-and-compliance)
- [See also](https://code.claude.com/docs/en/slash-commands#see-also)
- [Service partners](https://claude.com/partners/services)
- [Skills vs slash commands](https://code.claude.com/docs/en/slash-commands#skills-vs-slash-commands)
- [Skip to main content](https://code.claude.com/docs/en/slash-commands#content-area)
- [Slash commands](https://code.claude.com/docs/en/slash-commands)
- [SlashCommand permission rules](https://code.claude.com/docs/en/slash-commands#slashcommand-permission-rules)
- [SlashCommand tool](https://code.claude.com/docs/en/slash-commands#slashcommand-tool)
- [SlashCommand tool supported commands](https://code.claude.com/docs/en/slash-commands#slashcommand-tool-supported-commands)
- [Startups program](https://claude.com/programs/startups)
- [Status](https://status.anthropic.com/)
- [Support center](https://support.claude.com/)
- [Syntax](https://code.claude.com/docs/en/slash-commands#syntax)
- [Thinking mode](https://code.claude.com/docs/en/slash-commands#thinking-mode)
- [Transparency](https://www.anthropic.com/transparency)
- [Trust center](https://trust.anthropic.com/)
- [Usage policy](https://www.anthropic.com/legal/aup)
- [Use Skills for](https://code.claude.com/docs/en/slash-commands#use-skills-for)
- [Use slash commands for](https://code.claude.com/docs/en/slash-commands#use-slash-commands-for)
- [When to use each](https://code.claude.com/docs/en/slash-commands#when-to-use-each)
- [x](https://x.com/AnthropicAI)
