---
title: "Plugin marketplaces - Claude Code Docs"
source_url: "https://code.claude.com/docs/en/plugin-marketplaces"
word_count: 1862
reading_time: "10 min read"
date_converted: "2025-12-01T17:33:42.444Z"
---
# Plugin marketplaces - Claude Code Docs

Plugin marketplaces are catalogs of available plugins that make it easy to discover, install, and manage Claude Code extensions. This guide shows you how to use existing marketplaces and create your own for team distribution.

Overview
--------

A marketplace is a JSON file that lists available plugins and describes where to find them. Marketplaces provide:

*   **Centralized discovery**: Browse plugins from multiple sources in one place
*   **Version management**: Track and update plugin versions automatically
*   **Team distribution**: Share required plugins across your organization
*   **Flexible sources**: Support for git repositories, GitHub repos, local paths, and package managers

### Prerequisites

*   Claude Code installed and running
*   Basic familiarity with JSON file format
*   For creating marketplaces: Git repository or local development environment

Add and use marketplaces
------------------------

Add marketplaces using the `/plugin marketplace` commands to access plugins from different sources:

### Add GitHub marketplaces

Add a GitHub repository containing .claude-plugin/marketplace.json

```
/plugin marketplace add owner/repo
```

### Add Git repositories

Add any git repository

```
/plugin marketplace add https://gitlab.com/company/plugins.git
```

### Add local marketplaces for development

Add local directory containing .claude-plugin/marketplace.json

```
/plugin marketplace add ./my-marketplace
```

Add direct path to marketplace.json file

```
/plugin marketplace add ./path/to/marketplace.json
```

Add remote marketplace.json via URL

```
/plugin marketplace add https://url.of/marketplace.json
```

### Install plugins from marketplaces

Once you’ve added marketplaces, install plugins directly:

Install from any known marketplace

```
/plugin install plugin-name@marketplace-name
```

Browse available plugins interactively

```
/plugin
```

### Verify marketplace installation

After adding a marketplace:

1.   **List marketplaces**: Run `/plugin marketplace list` to confirm it’s added
2.   **Browse plugins**: Use `/plugin` to see available plugins from your marketplace
3.   **Test installation**: Try installing a plugin to verify the marketplace works correctly

### Example plugin marketplace

Claude Code maintains a marketplace of [demo plugins](https://github.com/anthropics/claude-code/tree/main/plugins). These plugins are examples of what’s possible with the plugin system.

Add the marketplace

```
/plugin marketplace add anthropics/claude-code
```

Configure team marketplaces
---------------------------

Set up automatic marketplace installation for team projects by specifying required marketplaces in `.claude/settings.json`:

```
{
  "extraKnownMarketplaces": {
    "team-tools": {
      "source": {
        "source": "github",
        "repo": "your-org/claude-plugins"
      }
    },
    "project-specific": {
      "source": {
        "source": "git",
        "url": "https://git.company.com/project-plugins.git"
      }
    }
  }
}
```

When team members trust the repository folder, Claude Code automatically installs these marketplaces and any plugins specified in the `enabledPlugins` field.

* * *

Create your own marketplace
---------------------------

Build and distribute custom plugin collections for your team or community.

### Prerequisites for marketplace creation

*   Git repository (GitHub, GitLab, or other git hosting)
*   Understanding of JSON file format
*   One or more plugins to distribute

### Create the marketplace file

Create `.claude-plugin/marketplace.json` in your repository root:

```
{
  "name": "company-tools",
  "owner": {
    "name": "DevTools Team",
    "email": "[email protected]"
  },
  "plugins": [
    {
      "name": "code-formatter",
      "source": "./plugins/formatter",
      "description": "Automatic code formatting on save",
      "version": "2.1.0",
      "author": {
        "name": "DevTools Team"
      }
    },
    {
      "name": "deployment-tools",
      "source": {
        "source": "github",
        "repo": "company/deploy-plugin"
      },
      "description": "Deployment automation tools"
    }
  ]
}
```

### Marketplace schema

#### Required fields

| Field | Type | Description |
| --- | --- | --- |
| `name` | string | Marketplace identifier (kebab-case, no spaces) |
| `owner` | object | Marketplace maintainer information |
| `plugins` | array | List of available plugins |

#### Optional metadata

| Field | Type | Description |
| --- | --- | --- |
| `metadata.description` | string | Brief marketplace description |
| `metadata.version` | string | Marketplace version |
| `metadata.pluginRoot` | string | Base path for relative plugin sources |

### Plugin entries

**Required fields:**

| Field | Type | Description |
| --- | --- | --- |
| `name` | string | Plugin identifier (kebab-case, no spaces) |
| `source` | string|object | Where to fetch the plugin from |

#### Optional plugin fields

**Standard metadata fields:**

| Field | Type | Description |
| --- | --- | --- |
| `description` | string | Brief plugin description |
| `version` | string | Plugin version |
| `author` | object | Plugin author information |
| `homepage` | string | Plugin homepage or documentation URL |
| `repository` | string | Source code repository URL |
| `license` | string | SPDX license identifier (e.g., MIT, Apache-2.0) |
| `keywords` | array | Tags for plugin discovery and categorization |
| `category` | string | Plugin category for organization |
| `tags` | array | Tags for searchability |
| `strict` | boolean | Require plugin.json in plugin folder (default: true) 1 |

**Component configuration fields:**

| Field | Type | Description |
| --- | --- | --- |
| `commands` | string|array | Custom paths to command files or directories |
| `agents` | string|array | Custom paths to agent files |
| `hooks` | string|object | Custom hooks configuration or path to hooks file |
| `mcpServers` | string|object | MCP server configurations or path to MCP config |

_1 - When `strict: true` (default), the plugin must include a `plugin.json` manifest file, and marketplace fields supplement those values. When `strict: false`, the plugin.json is optional. If it’s missing, the marketplace entry serves as the complete plugin manifest._

### Plugin sources

#### Relative paths

For plugins in the same repository:

```
{
  "name": "my-plugin",
  "source": "./plugins/my-plugin"
}
```

#### GitHub repositories

```
{
  "name": "github-plugin",
  "source": {
    "source": "github",
    "repo": "owner/plugin-repo"
  }
}
```

#### Git repositories

```
{
  "name": "git-plugin",
  "source": {
    "source": "url",
    "url": "https://gitlab.com/team/plugin.git"
  }
}
```

#### Advanced plugin entries

Plugin entries can override default component locations and provide additional metadata. Note that `${CLAUDE_PLUGIN_ROOT}` is an environment variable that resolves to the plugin’s installation directory (for details see [Environment variables](https://code.claude.com/docs/en/plugins-reference#environment-variables)):

```
{
  "name": "enterprise-tools",
  "source": {
    "source": "github",
    "repo": "company/enterprise-plugin"
  },
  "description": "Enterprise workflow automation tools",
  "version": "2.1.0",
  "author": {
    "name": "Enterprise Team",
    "email": "[email protected]"
  },
  "homepage": "https://docs.company.com/plugins/enterprise-tools",
  "repository": "https://github.com/company/enterprise-plugin",
  "license": "MIT",
  "keywords": ["enterprise", "workflow", "automation"],
  "category": "productivity",
  "commands": [
    "./commands/core/",
    "./commands/enterprise/",
    "./commands/experimental/preview.md"
  ],
  "agents": ["./agents/security-reviewer.md", "./agents/compliance-checker.md"],
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "${CLAUDE_PLUGIN_ROOT}/scripts/validate.sh"
          }
        ]
      }
    ]
  },
  "mcpServers": {
    "enterprise-db": {
      "command": "${CLAUDE_PLUGIN_ROOT}/servers/db-server",
      "args": ["--config", "${CLAUDE_PLUGIN_ROOT}/config.json"]
    }
  },
  "strict": false
}
```

* * *

Host and distribute marketplaces
--------------------------------

Choose the best hosting strategy for your plugin distribution needs.

### Host on GitHub (recommended)

GitHub provides the easiest distribution method:

1.   **Create a repository**: Set up a new repository for your marketplace
2.   **Add marketplace file**: Create `.claude-plugin/marketplace.json` with your plugin definitions
3.   **Share with teams**: Team members add with `/plugin marketplace add owner/repo`

**Benefits**: Built-in version control, issue tracking, and team collaboration features.

### Host on other git services

Any git hosting service works for marketplace distribution, using a URL to an arbitrary git repository.For example, using GitLab:

```
/plugin marketplace add https://gitlab.com/company/plugins.git
```

### Use local marketplaces for development

Test your marketplace locally before distribution:

Add local marketplace for testing

```
/plugin marketplace add ./my-local-marketplace
```

Test plugin installation

```
/plugin install test-plugin@my-local-marketplace
```

Manage marketplace operations
-----------------------------

### List known marketplaces

List all configured marketplaces

```
/plugin marketplace list
```

Shows all configured marketplaces with their sources and status.

### Update marketplace metadata

Refresh marketplace metadata

```
/plugin marketplace update marketplace-name
```

Refreshes plugin listings and metadata from the marketplace source.

### Remove a marketplace

Remove a marketplace

```
/plugin marketplace remove marketplace-name
```

Removes the marketplace from your configuration.

* * *

Troubleshooting marketplaces
----------------------------

### Common marketplace issues

#### Marketplace not loading

**Symptoms**: Can’t add marketplace or see plugins from it**Solutions**:

*   Verify the marketplace URL is accessible
*   Check that `.claude-plugin/marketplace.json` exists at the specified path
*   Ensure JSON syntax is valid using `claude plugin validate`
*   For private repositories, confirm you have access permissions

#### Plugin installation failures

**Symptoms**: Marketplace appears but plugin installation fails**Solutions**:

*   Verify plugin source URLs are accessible
*   Check that plugin directories contain required files
*   For GitHub sources, ensure repositories are public or you have access
*   Test plugin sources manually by cloning/downloading

### Validation and testing

Test your marketplace before sharing:

Validate marketplace JSON syntax

```
claude plugin validate .
```

Add marketplace for testing

```
/plugin marketplace add ./path/to/marketplace
```

Install test plugin

```
/plugin install test-plugin@marketplace-name
```

For complete plugin testing workflows, see [Test your plugins locally](https://code.claude.com/docs/en/plugins#test-your-plugins-locally). For technical troubleshooting, see [Plugins reference](https://code.claude.com/docs/en/plugins-reference).

* * *

Next steps
----------

### For marketplace users

*   **Discover community marketplaces**: Search GitHub for Claude Code plugin collections
*   **Contribute feedback**: Report issues and suggest improvements to marketplace maintainers
*   **Share useful marketplaces**: Help your team discover valuable plugin collections

### For marketplace creators

*   **Build plugin collections**: Create themed marketplace around specific use cases
*   **Establish versioning**: Implement clear versioning and update policies
*   **Community engagement**: Gather feedback and maintain active marketplace communities
*   **Documentation**: Provide clear README files explaining your marketplace contents

### For organizations

*   **Private marketplaces**: Set up internal marketplaces for proprietary tools
*   **Governance policies**: Establish guidelines for plugin approval and security review
*   **Training resources**: Help teams discover and adopt useful plugins effectively

See also
--------

*   [Plugins](https://code.claude.com/docs/en/plugins) - Installing and using plugins
*   [Plugins reference](https://code.claude.com/docs/en/plugins-reference) - Complete technical specifications and schemas
*   [Plugin development](https://code.claude.com/docs/en/plugins#develop-more-complex-plugins) - Creating your own plugins
*   [Settings](https://code.claude.com/docs/en/settings#plugin-configuration) - Plugin configuration options

## Links

- [[email protected]](https://code.claude.com/cdn-cgi/l/email-protection)
- [Add and use marketplaces](https://code.claude.com/docs/en/plugin-marketplaces#add-and-use-marketplaces)
- [Add Git repositories](https://code.claude.com/docs/en/plugin-marketplaces#add-git-repositories)
- [Add GitHub marketplaces](https://code.claude.com/docs/en/plugin-marketplaces#add-github-marketplaces)
- [Add local marketplaces for development](https://code.claude.com/docs/en/plugin-marketplaces#add-local-marketplaces-for-development)
- [Administration](https://code.claude.com/docs/en/setup)
- [Advanced plugin entries](https://code.claude.com/docs/en/plugin-marketplaces#advanced-plugin-entries)
- [Analytics](https://code.claude.com/docs/en/analytics)
- [Anthropic](https://www.anthropic.com/company)
- [Availability](https://www.anthropic.com/supported-countries)
- [Build with Claude Code](https://code.claude.com/docs/en/sub-agents)
- [Careers](https://www.anthropic.com/careers)
- [Claude Code Docs home page](https://code.claude.com/docs)
- [Claude Code on the Web](https://claude.ai/code)
- [Claude Developer Platform](https://platform.claude.com/)
- [Commercial terms](https://www.anthropic.com/legal/commercial-terms)
- [Common marketplace issues](https://code.claude.com/docs/en/plugin-marketplaces#common-marketplace-issues)
- [Configuration](https://code.claude.com/docs/en/settings)
- [Configure team marketplaces](https://code.claude.com/docs/en/plugin-marketplaces#configure-team-marketplaces)
- [Consumer terms](https://www.anthropic.com/legal/consumer-terms)
- [Costs](https://code.claude.com/docs/en/costs)
- [Courses](https://www.anthropic.com/learn)
- [Create the marketplace file](https://code.claude.com/docs/en/plugin-marketplaces#create-the-marketplace-file)
- [Create your own marketplace](https://code.claude.com/docs/en/plugin-marketplaces#create-your-own-marketplace)
- [Customer stories](https://www.claude.com/customers)
- [Data usage](https://code.claude.com/docs/en/data-usage)
- [demo plugins](https://github.com/anthropics/claude-code/tree/main/plugins)
- [Deployment](https://code.claude.com/docs/en/third-party-integrations)
- [Disclosure policy](https://www.anthropic.com/responsible-disclosure-policy)
- [Economic Futures](https://www.anthropic.com/economic-futures)
- [Engineering blog](https://www.anthropic.com/engineering)
- [Environment variables](https://code.claude.com/docs/en/plugins-reference#environment-variables)
- [Events](https://www.anthropic.com/events)
- [Example plugin marketplace](https://code.claude.com/docs/en/plugin-marketplaces#example-plugin-marketplace)
- [For marketplace creators](https://code.claude.com/docs/en/plugin-marketplaces#for-marketplace-creators)
- [For marketplace users](https://code.claude.com/docs/en/plugin-marketplaces#for-marketplace-users)
- [For organizations](https://code.claude.com/docs/en/plugin-marketplaces#for-organizations)
- [Getting started](https://code.claude.com/docs/en/overview)
- [Git repositories](https://code.claude.com/docs/en/plugin-marketplaces#git-repositories)
- [GitHub repositories](https://code.claude.com/docs/en/plugin-marketplaces#github-repositories)
- [Host and distribute marketplaces](https://code.claude.com/docs/en/plugin-marketplaces#host-and-distribute-marketplaces)
- [Host on GitHub (recommended)](https://code.claude.com/docs/en/plugin-marketplaces#host-on-github-recommended)
- [Host on other git services](https://code.claude.com/docs/en/plugin-marketplaces#host-on-other-git-services)
- [Identity and Access Management](https://code.claude.com/docs/en/iam)
- [Install plugins from marketplaces](https://code.claude.com/docs/en/plugin-marketplaces#install-plugins-from-marketplaces)
- [linkedin](https://www.linkedin.com/company/anthropicresearch)
- [List known marketplaces](https://code.claude.com/docs/en/plugin-marketplaces#list-known-marketplaces)
- [Manage marketplace operations](https://code.claude.com/docs/en/plugin-marketplaces#manage-marketplace-operations)
- [Marketplace not loading](https://code.claude.com/docs/en/plugin-marketplaces#marketplace-not-loading)
- [Marketplace schema](https://code.claude.com/docs/en/plugin-marketplaces#marketplace-schema)
- [MCP connectors](https://claude.com/partners/mcp)
- [Monitoring](https://code.claude.com/docs/en/monitoring-usage)
- [News](https://www.anthropic.com/news)
- [Next steps](https://code.claude.com/docs/en/plugin-marketplaces#next-steps)
- [Optional metadata](https://code.claude.com/docs/en/plugin-marketplaces#optional-metadata)
- [Optional plugin fields](https://code.claude.com/docs/en/plugin-marketplaces#optional-plugin-fields)
- [Overview](https://code.claude.com/docs/en/plugin-marketplaces#overview)
- [Plugin development](https://code.claude.com/docs/en/plugins#develop-more-complex-plugins)
- [Plugin entries](https://code.claude.com/docs/en/plugin-marketplaces#plugin-entries)
- [Plugin installation failures](https://code.claude.com/docs/en/plugin-marketplaces#plugin-installation-failures)
- [Plugin marketplaces](https://code.claude.com/docs/en/plugin-marketplaces)
- [Plugin sources](https://code.claude.com/docs/en/plugin-marketplaces#plugin-sources)
- [Plugins](https://code.claude.com/docs/en/plugins)
- [Plugins reference](https://code.claude.com/docs/en/plugins-reference)
- [Powered by Claude](https://claude.com/partners/powered-by-claude)
- [Prerequisites](https://code.claude.com/docs/en/plugin-marketplaces#prerequisites)
- [Prerequisites for marketplace creation](https://code.claude.com/docs/en/plugin-marketplaces#prerequisites-for-marketplace-creation)
- [Privacy policy](https://www.anthropic.com/legal/privacy)
- [Reference](https://code.claude.com/docs/en/cli-reference)
- [Relative paths](https://code.claude.com/docs/en/plugin-marketplaces#relative-paths)
- [Remove a marketplace](https://code.claude.com/docs/en/plugin-marketplaces#remove-a-marketplace)
- [Required fields](https://code.claude.com/docs/en/plugin-marketplaces#required-fields)
- [Research](https://www.anthropic.com/research)
- [Resources](https://code.claude.com/docs/en/legal-and-compliance)
- [Security](https://code.claude.com/docs/en/security)
- [See also](https://code.claude.com/docs/en/plugin-marketplaces#see-also)
- [Service partners](https://claude.com/partners/services)
- [Settings](https://code.claude.com/docs/en/settings#plugin-configuration)
- [Skip to main content](https://code.claude.com/docs/en/plugin-marketplaces#content-area)
- [Startups program](https://claude.com/programs/startups)
- [Status](https://status.anthropic.com/)
- [Support center](https://support.claude.com/)
- [Test your plugins locally](https://code.claude.com/docs/en/plugins#test-your-plugins-locally)
- [Transparency](https://www.anthropic.com/transparency)
- [Troubleshooting marketplaces](https://code.claude.com/docs/en/plugin-marketplaces#troubleshooting-marketplaces)
- [Trust center](https://trust.anthropic.com/)
- [Update marketplace metadata](https://code.claude.com/docs/en/plugin-marketplaces#update-marketplace-metadata)
- [Usage policy](https://www.anthropic.com/legal/aup)
- [Use local marketplaces for development](https://code.claude.com/docs/en/plugin-marketplaces#use-local-marketplaces-for-development)
- [Validation and testing](https://code.claude.com/docs/en/plugin-marketplaces#validation-and-testing)
- [Verify marketplace installation](https://code.claude.com/docs/en/plugin-marketplaces#verify-marketplace-installation)
- [x](https://x.com/AnthropicAI)
