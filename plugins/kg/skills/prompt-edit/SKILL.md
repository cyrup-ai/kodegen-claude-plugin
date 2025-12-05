---
name: prompt-edit
description: >
  Edit an existing prompt template by replacing its entire content.
  WHEN: User wants to "edit a prompt", "update a template", "modify prompt content".
  WHEN NOT: Creating new prompt (use prompt_add), deleting (use prompt_delete).
version: 0.1.0
---

# prompt_edit - Modify Prompt Templates

## Core Concept

`mcp__plugin_kg_kodegen__prompt_edit` updates an existing prompt template. The new content completely replaces the old content. Always retrieve current content first with `prompt_get`.

## Key Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | Yes | Name of the prompt to edit |
| `content` | string | Yes | Complete new content (YAML frontmatter + body) |

## Workflow

### Step 1: Get Current Content
```json
{
  "action": "get",
  "name": "my_template"
}
```

### Step 2: Edit the Content
```json
{
  "name": "my_template",
  "content": "---\ntitle: \"Updated Title\"\ndescription: \"Updated description\"\ncategories: [\"updated\"]\nauthor: \"your-name\"\nparameters:\n  - name: \"new_param\"\n    required: true\n---\n\nUpdated template content using {{ new_param }}.\n"
}
```

## Usage Example

```json
{
  "name": "code_review",
  "content": "---\ntitle: \"Enhanced Code Review\"\ndescription: \"Review code with configurable depth\"\ncategories: [\"code\", \"review\"]\nauthor: \"team\"\nparameters:\n  - name: \"language\"\n    description: \"Programming language\"\n    required: true\n  - name: \"depth\"\n    description: \"Review depth\"\n    default: \"standard\"\n---\n\n# Code Review Request\n\nLanguage: {{ language | upper }}\nDepth: {{ depth }}\n\n{% if depth == 'thorough' %}\nPerform comprehensive review including:\n- Security analysis\n- Performance review\n- Best practices check\n{% else %}\nPerform standard code review.\n{% endif %}\n"
}
```

## Important Notes

- **Full replacement** - New content completely replaces old content
- **Validation** - Template syntax is validated before saving
- **Retrieve first** - Always use `prompt_get` to see current content before editing
- **Idempotent** - Same content produces same result

## Remember

- Content must include complete YAML frontmatter
- Always retrieve current content first with `prompt_get`
- The entire template is replaced - partial updates are not supported
- Syntax errors will be reported and the edit will fail
- Use this for modifications; use `prompt_add` for new templates
