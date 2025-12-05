---
name: browser-type-text
description: >
  Type text into form fields using CSS selectors.
  WHEN: User needs to fill out a form, enter text into an input field, type in a search box, enter credentials.
  WHEN NOT: User needs to click a button (use browser_click), user needs to read content (use browser_extract_text).
version: 0.1.0
---

# browser_type_text - Enter Text in Forms

## Core Concept

`mcp__plugin_kg_kodegen__browser_type_text` types text into form fields. Automatically:
- Focuses the element
- Scrolls into view
- Clears existing text (by default)
- Types character by character (realistic simulation)

Requires `browser_navigate` to be called first.

## Key Parameters

### selector (required)
CSS selector for the input element.

### text (required)
Text to type into the field.

### clear (optional, default: true)
Whether to clear existing text before typing.

```json
{"selector": "#field", "text": "append this", "clear": false}
```

### timeout_ms (optional, default: 5000)
Maximum time to wait for element.

## Usage Examples

### Basic Text Input
```json
browser_type_text({
  "selector": "#email",
  "text": "user@example.com"
})
```

### Search Box
```json
browser_type_text({
  "selector": "input[name='q']",
  "text": "rust programming"
})
```

### Append Text (No Clear)
```json
browser_type_text({
  "selector": "#notes",
  "text": "\nAdditional line",
  "clear": false
})
```

### Password Field
```json
browser_type_text({
  "selector": "input[type='password']",
  "text": "securepassword123"
})
```

## Common Patterns

### Login Form
```json
1. browser_navigate({"url": "https://example.com/login"})
2. browser_type_text({"selector": "#username", "text": "myuser"})
3. browser_type_text({"selector": "#password", "text": "mypass"})
4. browser_click({"selector": "#login-btn", "wait_for_navigation": true})
```

### Search Flow
```json
1. browser_navigate({"url": "https://example.com"})
2. browser_type_text({"selector": ".search-input", "text": "query"})
3. browser_click({"selector": ".search-btn"})
4. browser_extract_text({"selector": ".results"})
```

### Multi-Field Form
```json
browser_type_text({"selector": "input[name='firstName']", "text": "John"})
browser_type_text({"selector": "input[name='lastName']", "text": "Doe"})
browser_type_text({"selector": "input[type='email']", "text": "john@example.com"})
browser_type_text({"selector": "textarea[name='bio']", "text": "Developer..."})
```

## Selector Examples

| Field Type | Selector Pattern |
|------------|------------------|
| By ID | `#email` |
| By name | `input[name='username']` |
| By type | `input[type='password']` |
| By class | `.form-input` |
| By placeholder | `input[placeholder='Enter email']` |
| Nested | `form.login input[name='email']` |

## Troubleshooting

- **Element not found**: Verify selector, check if inside iframe (not supported)
- **Text not appearing**: Field may be read-only or disabled
- **Wrong field cleared**: Use `clear: false` to append
- **Validation errors**: Some fields have client-side validation

## Remember

- Call browser_navigate first
- Default clears existing text (use clear: false to append)
- Works with input, textarea, and contenteditable elements
- Element auto-focuses and scrolls into view
- Use browser_click after typing to submit forms
