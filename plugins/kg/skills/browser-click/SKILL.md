---
name: browser-click
description: >
  Click elements on a web page using CSS selectors.
  WHEN: User needs to click a button, link, checkbox, or any interactive element on a page.
  WHEN NOT: User needs to type text (use browser_type_text), user needs to scroll (use browser_scroll).
version: 0.1.0
---

# browser_click - Click Page Elements

## Core Concept

`mcp__plugin_kg_kodegen__browser_click` clicks elements on the current page using CSS selectors. Automatically:
- Scrolls element into view
- Waits for element to appear (SPA support)
- Handles timing for dynamic content

Requires `browser_navigate` to be called first.

## Key Parameters

### selector (required)
CSS selector for the element to click.

```json
{"selector": "#submit-button"}
{"selector": "button[type='submit']"}
{"selector": ".btn-primary"}
```

### timeout_ms (optional, default: 5000)
Maximum time to wait for element to appear.

### wait_for_navigation (optional, default: false)
Wait for page navigation after click. Use for submit buttons and links.

```json
{"selector": "a.nav-link", "wait_for_navigation": true}
```

## Usage Examples

### Click by ID
```json
browser_click({"selector": "#login-button"})
```

### Click by Class
```json
browser_click({"selector": ".submit-btn"})
```

### Click by Attribute
```json
browser_click({"selector": "button[data-action='submit']"})
```

### Click with Navigation Wait
```json
browser_click({
  "selector": "a.next-page",
  "wait_for_navigation": true
})
```

### Complex Selector
```json
browser_click({"selector": "form.login button[type='submit']"})
```

## CSS Selector Reference

| Pattern | Example | Matches |
|---------|---------|---------|
| ID | `#submit` | Element with id="submit" |
| Class | `.btn` | Elements with class="btn" |
| Attribute | `[type='submit']` | Elements with type="submit" |
| Tag + Class | `button.primary` | button elements with class="primary" |
| Descendant | `.form button` | button inside .form |
| Child | `.form > button` | Direct child button of .form |
| nth-child | `li:nth-child(3)` | Third li element |

## Common Patterns

### Form Submission
```json
1. browser_navigate({"url": "https://example.com/form"})
2. browser_type_text({"selector": "#email", "text": "user@example.com"})
3. browser_type_text({"selector": "#password", "text": "password"})
4. browser_click({"selector": "#submit", "wait_for_navigation": true})
```

### Modal Close
```json
browser_click({"selector": ".modal .close-btn"})
```

### Checkbox Toggle
```json
browser_click({"selector": "input[type='checkbox']#agree"})
```

## Troubleshooting

- **Element not found**: Verify selector, increase timeout_ms, check with screenshot
- **Element obscured**: Another element blocks click, scroll or dismiss overlay
- **Element disabled**: Check if button is disabled, may need to fill required fields first

## Remember

- Call browser_navigate first
- Element auto-scrolls into view before click
- Use wait_for_navigation for links/submit buttons
- Verify selectors with browser_screenshot
- 5 second default timeout for SPA elements
