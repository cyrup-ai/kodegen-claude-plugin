---
title: "Get started with Claude Code hooks - Claude Code Docs"
source_url: "https://code.claude.com/docs/en/hooks-guide"
word_count: 1250
reading_time: "7 min read"
date_converted: "2025-12-01T17:37:50.585Z"
---
# Get started with Claude Code hooks - Claude Code Docs

Claude Code hooks are user-defined shell commands that execute at various points in Claude Code’s lifecycle. Hooks provide deterministic control over Claude Code’s behavior, ensuring certain actions always happen rather than relying on the LLM to choose to run them.

Example use cases for hooks include:

*   **Notifications**: Customize how you get notified when Claude Code is awaiting your input or permission to run something.
*   **Automatic formatting**: Run `prettier` on .ts files, `gofmt` on .go files, etc. after every file edit.
*   **Logging**: Track and count all executed commands for compliance or debugging.
*   **Feedback**: Provide automated feedback when Claude Code produces code that does not follow your codebase conventions.
*   **Custom permissions**: Block modifications to production files or sensitive directories.

By encoding these rules as hooks rather than prompting instructions, you turn suggestions into app-level code that executes every time it is expected to run.

Hook Events Overview
--------------------

Claude Code provides several hook events that run at different points in the workflow:

*   **PreToolUse**: Runs before tool calls (can block them)
*   **PermissionRequest**: Runs when a permission dialog is shown (can allow or deny)
*   **PostToolUse**: Runs after tool calls complete
*   **UserPromptSubmit**: Runs when the user submits a prompt, before Claude processes it
*   **Notification**: Runs when Claude Code sends notifications
*   **Stop**: Runs when Claude Code finishes responding
*   **SubagentStop**: Runs when subagent tasks complete
*   **PreCompact**: Runs before Claude Code is about to run a compact operation
*   **SessionStart**: Runs when Claude Code starts a new session or resumes an existing session
*   **SessionEnd**: Runs when Claude Code session ends

Each event receives different data and can control Claude’s behavior in different ways.

Quickstart
----------

In this quickstart, you’ll add a hook that logs the shell commands that Claude Code runs.

### Prerequisites

Install `jq` for JSON processing in the command line.

### Step 1: Open hooks configuration

Run the `/hooks`[slash command](https://code.claude.com/docs/en/slash-commands) and select the `PreToolUse` hook event.`PreToolUse` hooks run before tool calls and can block them while providing Claude feedback on what to do differently.

### Step 2: Add a matcher

Select `+ Add new matcher…` to run your hook only on Bash tool calls.Type `Bash` for the matcher.

### Step 3: Add the hook

Select `+ Add new hook…` and enter this command:

```
jq -r '"\(.tool_input.command) - \(.tool_input.description // "No description")"' >> ~/.claude/bash-command-log.txt
```

### Step 4: Save your configuration

For storage location, select `User settings` since you’re logging to your home directory. This hook will then apply to all projects, not just your current project.Then press Esc until you return to the REPL. Your hook is now registered!

### Step 5: Verify your hook

Run `/hooks` again or check `~/.claude/settings.json` to see your configuration:

```
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "jq -r '\"\\(.tool_input.command) - \\(.tool_input.description // \"No description\")\"' >> ~/.claude/bash-command-log.txt"
          }
        ]
      }
    ]
  }
}
```

### Step 6: Test your hook

Ask Claude to run a simple command like `ls` and check your log file:

```
cat ~/.claude/bash-command-log.txt
```

You should see entries like:

```
ls - Lists files and directories
```

More Examples
-------------

### Code Formatting Hook

Automatically format TypeScript files after editing:

```
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "jq -r '.tool_input.file_path' | { read file_path; if echo \"$file_path\" | grep -q '\\.ts$'; then npx prettier --write \"$file_path\"; fi; }"
          }
        ]
      }
    ]
  }
}
```

### Markdown Formatting Hook

Automatically fix missing language tags and formatting issues in markdown files:

```
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR\"/.claude/hooks/markdown_formatter.py"
          }
        ]
      }
    ]
  }
}
```

Create `.claude/hooks/markdown_formatter.py` with this content:

```
#!/usr/bin/env python3
"""
Markdown formatter for Claude Code output.
Fixes missing language tags and spacing issues while preserving code content.
"""
import json
import sys
import re
import os

def detect_language(code):
    """Best-effort language detection from code content."""
    s = code.strip()
    
    # JSON detection
    if re.search(r'^\s*[{\[]', s):
        try:
            json.loads(s)
            return 'json'
        except:
            pass
    
    # Python detection
    if re.search(r'^\s*def\s+\w+\s*\(', s, re.M) or \
       re.search(r'^\s*(import|from)\s+\w+', s, re.M):
        return 'python'
    
    # JavaScript detection  
    if re.search(r'\b(function\s+\w+\s*\(|const\s+\w+\s*=)', s) or \
       re.search(r'=>|console\.(log|error)', s):
        return 'javascript'
    
    # Bash detection
    if re.search(r'^#!.*\b(bash|sh)\b', s, re.M) or \
       re.search(r'\b(if|then|fi|for|in|do|done)\b', s):
        return 'bash'
    
    # SQL detection
    if re.search(r'\b(SELECT|INSERT|UPDATE|DELETE|CREATE)\s+', s, re.I):
        return 'sql'
        
    return 'text'

def format_markdown(content):
    """Format markdown content with language detection."""
    # Fix unlabeled code fences
    def add_lang_to_fence(match):
        indent, info, body, closing = match.groups()
        if not info.strip():
            lang = detect_language(body)
            return f"{indent}```{lang}\n{body}{closing}\n"
        return match.group(0)
    
    fence_pattern = r'(?ms)^([ \t]{0,3})```([^\n]*)\n(.*?)(\n\1```)\s*$'
    content = re.sub(fence_pattern, add_lang_to_fence, content)
    
    # Fix excessive blank lines (only outside code fences)
    content = re.sub(r'\n{3,}', '\n\n', content)
    
    return content.rstrip() + '\n'

# Main execution
try:
    input_data = json.load(sys.stdin)
    file_path = input_data.get('tool_input', {}).get('file_path', '')
    
    if not file_path.endswith(('.md', '.mdx')):
        sys.exit(0)  # Not a markdown file
    
    if os.path.exists(file_path):
        with open(file_path, 'r', encoding='utf-8') as f:
            content = f.read()
        
        formatted = format_markdown(content)
        
        if formatted != content:
            with open(file_path, 'w', encoding='utf-8') as f:
                f.write(formatted)
            print(f"✓ Fixed markdown formatting in {file_path}")
    
except Exception as e:
    print(f"Error formatting markdown: {e}", file=sys.stderr)
    sys.exit(1)
```

Make the script executable:

```
chmod +x .claude/hooks/markdown_formatter.py
```

This hook automatically:

*   Detects programming languages in unlabeled code blocks
*   Adds appropriate language tags for syntax highlighting
*   Fixes excessive blank lines while preserving code content
*   Only processes markdown files (`.md`, `.mdx`)

### Custom Notification Hook

Get desktop notifications when Claude needs input:

```
{
  "hooks": {
    "Notification": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "notify-send 'Claude Code' 'Awaiting your input'"
          }
        ]
      }
    ]
  }
}
```

### File Protection Hook

Block edits to sensitive files:

```
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "python3 -c \"import json, sys; data=json.load(sys.stdin); path=data.get('tool_input',{}).get('file_path',''); sys.exit(2 if any(p in path for p in ['.env', 'package-lock.json', '.git/']) else 0)\""
          }
        ]
      }
    ]
  }
}
```

Learn more
----------

*   For reference documentation on hooks, see [Hooks reference](https://code.claude.com/docs/en/hooks).
*   For comprehensive security best practices and safety guidelines, see [Security Considerations](https://code.claude.com/docs/en/hooks#security-considerations) in the hooks reference documentation.
*   For troubleshooting steps and debugging techniques, see [Debugging](https://code.claude.com/docs/en/hooks#debugging) in the hooks reference documentation.

## Links

- [Administration](https://code.claude.com/docs/en/setup)
- [Agent Skills](https://code.claude.com/docs/en/skills)
- [Anthropic](https://www.anthropic.com/company)
- [Availability](https://www.anthropic.com/supported-countries)
- [bash command validator example](https://github.com/anthropics/claude-code/blob/main/examples/hooks/bash_command_validator_example.py)
- [Build with Claude Code](https://code.claude.com/docs/en/sub-agents)
- [Careers](https://www.anthropic.com/careers)
- [Claude Code Docs home page](https://code.claude.com/docs)
- [Claude Code on the Web](https://claude.ai/code)
- [Claude Developer Platform](https://platform.claude.com/)
- [Code Formatting Hook](https://code.claude.com/docs/en/hooks-guide#code-formatting-hook)
- [Commercial terms](https://www.anthropic.com/legal/commercial-terms)
- [Configuration](https://code.claude.com/docs/en/settings)
- [Consumer terms](https://www.anthropic.com/legal/consumer-terms)
- [Courses](https://www.anthropic.com/learn)
- [Custom Notification Hook](https://code.claude.com/docs/en/hooks-guide#custom-notification-hook)
- [Customer stories](https://www.claude.com/customers)
- [Debugging](https://code.claude.com/docs/en/hooks#debugging)
- [Deployment](https://code.claude.com/docs/en/third-party-integrations)
- [Disclosure policy](https://www.anthropic.com/responsible-disclosure-policy)
- [Economic Futures](https://www.anthropic.com/economic-futures)
- [Engineering blog](https://www.anthropic.com/engineering)
- [Events](https://www.anthropic.com/events)
- [File Protection Hook](https://code.claude.com/docs/en/hooks-guide#file-protection-hook)
- [Getting started](https://code.claude.com/docs/en/overview)
- [Headless mode](https://code.claude.com/docs/en/headless)
- [Hook Events Overview](https://code.claude.com/docs/en/hooks-guide#hook-events-overview)
- [Hooks](https://code.claude.com/docs/en/hooks-guide)
- [Hooks reference](https://code.claude.com/docs/en/hooks)
- [Learn more](https://code.claude.com/docs/en/hooks-guide#learn-more)
- [linkedin](https://www.linkedin.com/company/anthropicresearch)
- [Markdown Formatting Hook](https://code.claude.com/docs/en/hooks-guide#markdown-formatting-hook)
- [MCP connectors](https://claude.com/partners/mcp)
- [Migrate to Claude Agent SDK](https://code.claude.com/docs/en/sdk/migration-guide)
- [Model Context Protocol (MCP)](https://code.claude.com/docs/en/mcp)
- [More Examples](https://code.claude.com/docs/en/hooks-guide#more-examples)
- [News](https://www.anthropic.com/news)
- [Output styles](https://code.claude.com/docs/en/output-styles)
- [Plugins](https://code.claude.com/docs/en/plugins)
- [Powered by Claude](https://claude.com/partners/powered-by-claude)
- [Prerequisites](https://code.claude.com/docs/en/hooks-guide#prerequisites)
- [Privacy policy](https://www.anthropic.com/legal/privacy)
- [Quickstart](https://code.claude.com/docs/en/hooks-guide#quickstart)
- [Reference](https://code.claude.com/docs/en/cli-reference)
- [Research](https://www.anthropic.com/research)
- [Resources](https://code.claude.com/docs/en/legal-and-compliance)
- [Security Considerations](https://code.claude.com/docs/en/hooks#security-considerations)
- [Service partners](https://claude.com/partners/services)
- [Skip to main content](https://code.claude.com/docs/en/hooks-guide#content-area)
- [slash command](https://code.claude.com/docs/en/slash-commands)
- [Startups program](https://claude.com/programs/startups)
- [Status](https://status.anthropic.com/)
- [Step 1: Open hooks configuration](https://code.claude.com/docs/en/hooks-guide#step-1:-open-hooks-configuration)
- [Step 2: Add a matcher](https://code.claude.com/docs/en/hooks-guide#step-2:-add-a-matcher)
- [Step 3: Add the hook](https://code.claude.com/docs/en/hooks-guide#step-3:-add-the-hook)
- [Step 4: Save your configuration](https://code.claude.com/docs/en/hooks-guide#step-4:-save-your-configuration)
- [Step 5: Verify your hook](https://code.claude.com/docs/en/hooks-guide#step-5:-verify-your-hook)
- [Step 6: Test your hook](https://code.claude.com/docs/en/hooks-guide#step-6:-test-your-hook)
- [Support center](https://support.claude.com/)
- [Transparency](https://www.anthropic.com/transparency)
- [Troubleshooting](https://code.claude.com/docs/en/troubleshooting)
- [Trust center](https://trust.anthropic.com/)
- [Usage policy](https://www.anthropic.com/legal/aup)
- [x](https://x.com/AnthropicAI)
