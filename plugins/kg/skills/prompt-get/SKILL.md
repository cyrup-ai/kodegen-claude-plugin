---
name: prompt-get
description: >
  Browse, retrieve, and render prompt templates with 4 actions.
  WHEN: User wants to "list prompts", "get a template", "render a prompt", "see available prompts", "use a saved prompt".
  WHEN NOT: Creating prompts (use prompt_add), editing (use prompt_edit).
version: 0.1.0
---

# prompt_get - Browse and Render Prompts

## Core Concept

`mcp__plugin_kg_kodegen__prompt_get` provides read-only access to prompt templates with four actions: list categories, list prompts, get raw content, and render with parameters.

## Actions

| Action | Description | Required Parameters |
|--------|-------------|---------------------|
| `list_categories` | Show all prompt categories | None |
| `list_prompts` | List prompts (optionally filtered) | `category` (optional) |
| `get` | Get raw template content | `name` |
| `render` | Render template with values | `name`, `parameters` |

## Usage Examples

### List All Categories
```json
{ "action": "list_categories" }
```

### List Prompts in Category
```json
{ "action": "list_prompts", "category": "code" }
```

### List All Prompts
```json
{ "action": "list_prompts" }
```

### Get Template Content
```json
{ "action": "get", "name": "code_review" }
```

### Render Template
```json
{
  "action": "render",
  "name": "code_review",
  "parameters": {
    "language": "rust",
    "depth": "thorough"
  }
}
```

## Response Format

### list_categories Response
```json
{
  "categories": [
    { "name": "code", "count": 5 },
    { "name": "documentation", "count": 3 }
  ],
  "total": 2
}
```

### list_prompts Response
```json
{
  "prompts": [
    {
      "name": "code_review",
      "title": "Code Review",
      "description": "Review code for issues",
      "categories": ["code"],
      "parameters": [
        { "name": "language", "required": true }
      ]
    }
  ],
  "count": 1
}
```

### get Response
Returns full template with metadata and raw Jinja2 content.

### render Response
Returns the final rendered text with all variables substituted.

## Common Workflows

**Discover and use a prompt:**
1. `list_categories` - See what's available
2. `list_prompts` with category - Find specific prompt
3. `get` - Understand parameters
4. `render` - Generate final output

**Quick render (known template):**
- `render` directly with name and parameters

## Remember

- Read-only tool - does not modify templates
- All operations are idempotent
- Use `get` to inspect parameters before `render`
- Check `verified` flag for production-ready prompts
- Parameters must match template definitions
