---
title: "Plugins - Claude Code Docs"
source_url: "https://code.claude.com/docs/en/plugins"
word_count: 1264
reading_time: "7 min read"
date_converted: "2025-12-01T17:35:39.313Z"
---
# Plugins - Claude Code Docs

Plugins let you extend Claude Code with custom functionality that can be shared across projects and teams. Install plugins from [marketplaces](https://code.claude.com/docs/en/plugin-marketplaces) to add pre-built commands, agents, hooks, Skills, and MCP servers, or create your own to automate your workflows.

Quickstart
----------

Let’s create a simple greeting plugin to get you familiar with the plugin system. We’ll build a working plugin that adds a custom command, test it locally, and understand the core concepts.

### Prerequisites

*   Claude Code installed on your machine
*   Basic familiarity with command-line tools

### Create your first plugin

1

2

3

4

5

6

You’ve successfully created and tested a plugin with these key components:

*   **Plugin manifest** (`.claude-plugin/plugin.json`) - Describes your plugin’s metadata
*   **Commands directory** (`commands/`) - Contains your custom slash commands
*   **Test marketplace** - Allows you to test your plugin locally

### Plugin structure overview

Your plugin follows this basic structure:

```
my-first-plugin/
├── .claude-plugin/
│   └── plugin.json          # Plugin metadata
├── commands/                 # Custom slash commands (optional)
│   └── hello.md
├── agents/                   # Custom agents (optional)
│   └── helper.md
├── skills/                   # Agent Skills (optional)
│   └── my-skill/
│       └── SKILL.md
└── hooks/                    # Event handlers (optional)
    └── hooks.json
```

**Additional components you can add:**

*   **Commands**: Create markdown files in `commands/` directory
*   **Agents**: Create agent definitions in `agents/` directory
*   **Skills**: Create `SKILL.md` files in `skills/` directory
*   **Hooks**: Create `hooks/hooks.json` for event handling
*   **MCP servers**: Create `.mcp.json` for external tool integration

* * *

Install and manage plugins
--------------------------

Learn how to discover, install, and manage plugins to extend your Claude Code capabilities.

### Prerequisites

*   Claude Code installed and running
*   Basic familiarity with command-line interfaces

### Add marketplaces

Marketplaces are catalogs of available plugins. Add them to discover and install plugins:

Add a marketplace

```
/plugin marketplace add your-org/claude-plugins
```

Browse available plugins

```
/plugin
```

For detailed marketplace management including Git repositories, local development, and team distribution, see [Plugin marketplaces](https://code.claude.com/docs/en/plugin-marketplaces).

### Install plugins

Open the plugin management interface

```
/plugin
```

Select “Browse Plugins” to see available options with descriptions, features, and installation options.

#### Via direct commands (for quick installation)

Install a specific plugin

```
/plugin install formatter@your-org
```

Enable a disabled plugin

```
/plugin enable plugin-name@marketplace-name
```

Disable without uninstalling

```
/plugin disable plugin-name@marketplace-name
```

Completely remove a plugin

```
/plugin uninstall plugin-name@marketplace-name
```

### Verify installation

After installing a plugin:

1.   **Check available commands**: Run `/help` to see new commands
2.   **Test plugin features**: Try the plugin’s commands and features
3.   **Review plugin details**: Use `/plugin` → “Manage Plugins” to see what the plugin provides

Set up team plugin workflows
----------------------------

Configure plugins at the repository level to ensure consistent tooling across your team. When team members trust your repository folder, Claude Code automatically installs specified marketplaces and plugins.**To set up team plugins:**

1.   Add marketplace and plugin configuration to your repository’s `.claude/settings.json`
2.   Team members trust the repository folder
3.   Plugins install automatically for all team members

For complete instructions including configuration examples, marketplace setup, and rollout best practices, see [Configure team marketplaces](https://code.claude.com/docs/en/plugin-marketplaces#how-to-configure-team-marketplaces).

* * *

Develop more complex plugins
----------------------------

Once you’re comfortable with basic plugins, you can create more sophisticated extensions.

### Add Skills to your plugin

Plugins can include [Agent Skills](https://code.claude.com/docs/en/skills) to extend Claude’s capabilities. Skills are model-invoked—Claude autonomously uses them based on the task context.To add Skills to your plugin, create a `skills/` directory at your plugin root and add Skill folders with `SKILL.md` files. Plugin Skills are automatically available when the plugin is installed.For complete Skill authoring guidance, see [Agent Skills](https://code.claude.com/docs/en/skills).

### Organize complex plugins

For plugins with many components, organize your directory structure by functionality. For complete directory layouts and organization patterns, see [Plugin directory structure](https://code.claude.com/docs/en/plugins-reference#plugin-directory-structure).

### Test your plugins locally

When developing plugins, use a local marketplace to test changes iteratively. This workflow builds on the quickstart pattern and works for plugins of any complexity.

1

2

3

4

### Debug plugin issues

If your plugin isn’t working as expected:

1.   **Check the structure**: Ensure your directories are at the plugin root, not inside `.claude-plugin/`
2.   **Test components individually**: Check each command, agent, and hook separately
3.   **Use validation and debugging tools**: See [Debugging and development tools](https://code.claude.com/docs/en/plugins-reference#debugging-and-development-tools) for CLI commands and troubleshooting techniques

When your plugin is ready to share:

1.   **Add documentation**: Include a README.md with installation and usage instructions
2.   **Version your plugin**: Use semantic versioning in your `plugin.json`
3.   **Create or use a marketplace**: Distribute through plugin marketplaces for easy installation
4.   **Test with others**: Have team members test the plugin before wider distribution

* * *

Next steps
----------

Now that you understand Claude Code’s plugin system, here are suggested paths for different goals:

### For plugin users

*   **Discover plugins**: Browse community marketplaces for useful tools
*   **Team adoption**: Set up repository-level plugins for your projects
*   **Marketplace management**: Learn to manage multiple plugin sources
*   **Advanced usage**: Explore plugin combinations and workflows

### For plugin developers

*   **Create your first marketplace**: [Plugin marketplaces guide](https://code.claude.com/docs/en/plugin-marketplaces)
*   **Advanced components**: Dive deeper into specific plugin components:
    *   [Slash commands](https://code.claude.com/docs/en/slash-commands) - Command development details
    *   [Subagents](https://code.claude.com/docs/en/sub-agents) - Agent configuration and capabilities
    *   [Agent Skills](https://code.claude.com/docs/en/skills) - Extend Claude’s capabilities
    *   [Hooks](https://code.claude.com/docs/en/hooks) - Event handling and automation
    *   [MCP](https://code.claude.com/docs/en/mcp) - External tool integration

*   **Distribution strategies**: Package and share your plugins effectively
*   **Community contribution**: Consider contributing to community plugin collections

### For team leads and administrators

*   **Repository configuration**: Set up automatic plugin installation for team projects
*   **Plugin governance**: Establish guidelines for plugin approval and security review
*   **Marketplace maintenance**: Create and maintain organization-specific plugin catalogs
*   **Training and documentation**: Help team members adopt plugin workflows effectively

See also
--------

*   [Plugin marketplaces](https://code.claude.com/docs/en/plugin-marketplaces) - Creating and managing plugin catalogs
*   [Slash commands](https://code.claude.com/docs/en/slash-commands) - Understanding custom commands
*   [Subagents](https://code.claude.com/docs/en/sub-agents) - Creating and using specialized agents
*   [Agent Skills](https://code.claude.com/docs/en/skills) - Extend Claude’s capabilities
*   [Hooks](https://code.claude.com/docs/en/hooks) - Automating workflows with event handlers
*   [MCP](https://code.claude.com/docs/en/mcp) - Connecting to external tools and services
*   [Settings](https://code.claude.com/docs/en/settings) - Configuration options for plugins

## Links

- [Add marketplaces](https://code.claude.com/docs/en/plugins#add-marketplaces)
- [Add Skills to your plugin](https://code.claude.com/docs/en/plugins#add-skills-to-your-plugin)
- [Administration](https://code.claude.com/docs/en/setup)
- [Agent Skills](https://code.claude.com/docs/en/skills)
- [Anthropic](https://www.anthropic.com/company)
- [Availability](https://www.anthropic.com/supported-countries)
- [Build with Claude Code](https://code.claude.com/docs/en/sub-agents)
- [Careers](https://www.anthropic.com/careers)
- [Claude Code Docs home page](https://code.claude.com/docs)
- [Claude Code on the Web](https://claude.ai/code)
- [Claude Developer Platform](https://platform.claude.com/)
- [Commercial terms](https://www.anthropic.com/legal/commercial-terms)
- [Configuration](https://code.claude.com/docs/en/settings)
- [Configure team marketplaces](https://code.claude.com/docs/en/plugin-marketplaces#how-to-configure-team-marketplaces)
- [Consumer terms](https://www.anthropic.com/legal/consumer-terms)
- [Courses](https://www.anthropic.com/learn)
- [Create your first plugin](https://code.claude.com/docs/en/plugins#create-your-first-plugin)
- [Customer stories](https://www.claude.com/customers)
- [Debug plugin issues](https://code.claude.com/docs/en/plugins#debug-plugin-issues)
- [Debugging and development tools](https://code.claude.com/docs/en/plugins-reference#debugging-and-development-tools)
- [Deployment](https://code.claude.com/docs/en/third-party-integrations)
- [Develop more complex plugins](https://code.claude.com/docs/en/plugins#develop-more-complex-plugins)
- [Disclosure policy](https://www.anthropic.com/responsible-disclosure-policy)
- [Economic Futures](https://www.anthropic.com/economic-futures)
- [Engineering blog](https://www.anthropic.com/engineering)
- [Events](https://www.anthropic.com/events)
- [For plugin developers](https://code.claude.com/docs/en/plugins#for-plugin-developers)
- [For plugin users](https://code.claude.com/docs/en/plugins#for-plugin-users)
- [For team leads and administrators](https://code.claude.com/docs/en/plugins#for-team-leads-and-administrators)
- [Getting started](https://code.claude.com/docs/en/overview)
- [Headless mode](https://code.claude.com/docs/en/headless)
- [Hooks](https://code.claude.com/docs/en/hooks)
- [Install and manage plugins](https://code.claude.com/docs/en/plugins#install-and-manage-plugins)
- [Install plugins](https://code.claude.com/docs/en/plugins#install-plugins)
- [linkedin](https://www.linkedin.com/company/anthropicresearch)
- [MCP connectors](https://claude.com/partners/mcp)
- [Migrate to Claude Agent SDK](https://code.claude.com/docs/en/sdk/migration-guide)
- [Model Context Protocol (MCP)](https://code.claude.com/docs/en/mcp)
- [News](https://www.anthropic.com/news)
- [Next steps](https://code.claude.com/docs/en/plugins#next-steps)
- [Organize complex plugins](https://code.claude.com/docs/en/plugins#organize-complex-plugins)
- [Output styles](https://code.claude.com/docs/en/output-styles)
- [Plugin directory structure](https://code.claude.com/docs/en/plugins-reference#plugin-directory-structure)
- [Plugin marketplaces](https://code.claude.com/docs/en/plugin-marketplaces)
- [Plugin sources](https://code.claude.com/docs/en/plugin-marketplaces#plugin-sources)
- [Plugin structure overview](https://code.claude.com/docs/en/plugins#plugin-structure-overview)
- [Plugins](https://code.claude.com/docs/en/plugins)
- [Plugins reference](https://code.claude.com/docs/en/plugins-reference)
- [Powered by Claude](https://claude.com/partners/powered-by-claude)
- [Prerequisites](https://code.claude.com/docs/en/plugins#prerequisites-2)
- [Privacy policy](https://www.anthropic.com/legal/privacy)
- [Quickstart](https://code.claude.com/docs/en/plugins#quickstart)
- [Reference](https://code.claude.com/docs/en/cli-reference)
- [Research](https://www.anthropic.com/research)
- [Resources](https://code.claude.com/docs/en/legal-and-compliance)
- [See also](https://code.claude.com/docs/en/plugins#see-also)
- [Service partners](https://claude.com/partners/services)
- [Set up team plugin workflows](https://code.claude.com/docs/en/plugins#set-up-team-plugin-workflows)
- [Share your plugins](https://code.claude.com/docs/en/plugins#share-your-plugins)
- [Skip to main content](https://code.claude.com/docs/en/plugins#content-area)
- [Slash commands](https://code.claude.com/docs/en/slash-commands)
- [Startups program](https://claude.com/programs/startups)
- [Status](https://status.anthropic.com/)
- [Support center](https://support.claude.com/)
- [Test your plugins locally](https://code.claude.com/docs/en/plugins#test-your-plugins-locally)
- [Transparency](https://www.anthropic.com/transparency)
- [Troubleshooting](https://code.claude.com/docs/en/troubleshooting)
- [Trust center](https://trust.anthropic.com/)
- [Usage policy](https://www.anthropic.com/legal/aup)
- [Verify installation](https://code.claude.com/docs/en/plugins#verify-installation)
- [Via direct commands (for quick installation)](https://code.claude.com/docs/en/plugins#via-direct-commands-for-quick-installation)
- [Via interactive menu (recommended for discovery)](https://code.claude.com/docs/en/plugins#via-interactive-menu-recommended-for-discovery)
- [x](https://x.com/AnthropicAI)
