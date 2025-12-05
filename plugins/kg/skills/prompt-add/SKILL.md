---
name: prompt-add
description: >
  Create reusable Jinja2 prompt templates with YAML frontmatter metadata.
  WHEN: User wants to "create a prompt", "save a template", "add a new prompt", "make a reusable prompt".
  WHEN NOT: Using existing prompt (use prompt_get), editing prompt (use prompt_edit).
version: 0.1.0
---

# prompt_add - Create Prompt Templates

## Core Concept

`mcp__plugin_kg_kodegen__prompt_add` creates new prompt templates stored at `~/.kodegen/prompts/`. Templates use Jinja2 syntax with YAML frontmatter for metadata. Content is validated before saving.

## Key Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | Yes | Template name (becomes filename without extension) |
| `content` | string | Yes | Full template with YAML frontmatter + Jinja2 body |

## Template Structure

Templates require YAML frontmatter followed by Jinja2 content:

```yaml
---
title: "Template Title"
description: "What this template does"
categories: ["category1", "category2"]
author: "your-name"
parameters:
  - name: "param_name"
    description: "What this parameter is for"
    required: true
  - name: "optional_param"
    description: "Optional parameter"
    required: false
    default: "default_value"
---

# Template Content

Use {{ param_name }} for variable substitution.
Use {% if optional_param %}conditional content{% endif %}.
Access environment: {{ env.USER }}, {{ env.HOME }}.
```

## Usage Examples

### Basic Template
```json
{
  "name": "code_review",
  "content": "---\ntitle: \"Code Review\"\ndescription: \"Review code for issues\"\ncategories: [\"code\"]\nauthor: \"team\"\nparameters:\n  - name: \"language\"\n    required: true\n---\n\nReview this {{ language }} code for bugs and best practices.\n"
}
```

### Template with Conditionals
```json
{
  "name": "analysis",
  "content": "---\ntitle: \"Analysis Template\"\nparameters:\n  - name: \"depth\"\n    default: \"standard\"\n  - name: \"focus_areas\"\n    required: false\n---\n\nAnalysis depth: {{ depth }}\n\n{% if depth == 'deep' %}\nPerform comprehensive analysis.\n{% else %}\nPerform standard analysis.\n{% endif %}\n\n{% if focus_areas %}\nFocus on:\n{% for area in focus_areas %}\n- {{ area }}\n{% endfor %}\n{% endif %}\n"
}
```

## Jinja2 Features

- `{{ variable }}` - Variable substitution
- `{% if condition %}...{% endif %}` - Conditionals
- `{% for item in list %}...{% endfor %}` - Loops
- `{{ var | upper }}`, `{{ var | lower }}`, `{{ var | title }}` - Filters
- `{{ env.VAR }}` - Environment variables
- `loop.index` (1-based), `loop.index0` (0-based) - Loop indices

## Remember

- Content MUST include YAML frontmatter (between `---` markers)
- Template syntax is validated before saving - errors are reported
- Templates saved to `~/.kodegen/prompts/{name}.j2.md`
- Use `prompt_get` to list existing templates before creating duplicates
- Parameters with `required: false` should have a `default` value
