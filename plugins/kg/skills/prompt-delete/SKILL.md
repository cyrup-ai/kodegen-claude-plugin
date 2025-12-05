---
name: prompt-delete
description: >
  Delete a prompt template permanently. Requires explicit confirmation.
  WHEN: User wants to "delete a prompt", "remove a template", "cleanup prompts".
  WHEN NOT: Editing prompt (use prompt_edit), temporarily disabling (no such feature).
version: 0.1.0
---

# prompt_delete - Remove Prompt Templates

## Core Concept

`mcp__plugin_kg_kodegen__prompt_delete` permanently removes a prompt template. This action cannot be undone. Default prompts will be recreated on next initialization.

## Key Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | Yes | Name of the prompt to delete |
| `confirm` | boolean | Yes | Must be `true` to proceed |

## Usage Examples

### Delete a Prompt
```json
{
  "name": "old_template",
  "confirm": true
}
```

### This Will Fail (safety check)
```json
{
  "name": "my_prompt",
  "confirm": false
}
```
Error: "Must set confirm=true to delete a prompt"

## Important Notes

- **Permanent deletion** - Cannot be undone
- **Confirmation required** - `confirm: true` is mandatory
- **Default prompts** - If you delete a default prompt, it will be recreated on next server restart
- **Verify first** - Use `prompt_get` with `action: "list_prompts"` to see existing prompts

## Workflow

1. List prompts: `prompt_get({ action: "list_prompts" })`
2. Verify the prompt to delete exists
3. Delete: `prompt_delete({ name: "target_prompt", confirm: true })`

## Remember

- Always set `confirm: true` - the tool will reject deletion otherwise
- This is a destructive operation - no recovery possible
- Use `prompt_get` to verify prompt exists before attempting deletion
- Default system prompts regenerate on restart
