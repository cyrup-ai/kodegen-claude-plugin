---
name: browser-screenshot
description: >
  Capture screenshots of web pages or specific elements.
  WHEN: User wants to see what a page looks like, verify visual appearance, capture page state, debug layout issues.
  WHEN NOT: User needs text content (use browser_extract_text), user needs to interact with page (use browser_click).
version: 0.1.0
---

# browser_screenshot - Capture Page Visuals

## Core Concept

`mcp__plugin_kg_kodegen__browser_screenshot` captures the current page or specific element as an image. Returns base64-encoded data suitable for:
- Vision model analysis
- Visual verification
- Layout debugging
- Documentation

Requires `browser_navigate` to be called first.

## Key Parameters

### selector (optional)
CSS selector for element screenshot. If omitted, captures full viewport.

```json
{}  // Full page
{"selector": "#hero-section"}  // Specific element
```

### format (optional, default: "png")
Image format: "png" (lossless) or "jpeg" (smaller size).

```json
{"format": "png"}   // Best for UI, text
{"format": "jpeg"}  // Best for photos, smaller files
```

## Usage Examples

### Full Page Screenshot
```json
browser_screenshot({})
```

### Element Screenshot
```json
browser_screenshot({"selector": ".modal-dialog"})
```

### JPEG Format (Smaller)
```json
browser_screenshot({"format": "jpeg"})
```

### Specific Component
```json
browser_screenshot({"selector": "#data-chart"})
```

## Response Format

```json
{
  "success": true,
  "width": 1920,
  "height": 1080,
  "format": "png",
  "base64": "iVBORw0KGgo..."
}
```

## Common Patterns

### Verify Page Load
```json
1. browser_navigate({"url": "https://example.com"})
2. browser_screenshot({})
// Vision model can analyze what's visible
```

### Debug Form State
```json
1. browser_navigate({"url": "..."})
2. browser_type_text({"selector": "#email", "text": "test"})
3. browser_screenshot({"selector": "form"})
// See form state after typing
```

### Before/After Interaction
```json
browser_screenshot({})  // Before
browser_click({"selector": ".expand-btn"})
browser_screenshot({})  // After
```

## Format Selection

| Use Case | Format | Why |
|----------|--------|-----|
| UI layouts | PNG | Sharp text and edges |
| Code blocks | PNG | Lossless for readability |
| Charts/diagrams | PNG | Clean lines |
| Photos | JPEG | Smaller file size |
| Quick previews | JPEG | Faster transfer |

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Visual verification | browser_screenshot | See actual appearance |
| Read text content | browser_extract_text | Structured text |
| Debug selectors | browser_screenshot | Visually confirm elements |
| Document page state | browser_screenshot | Visual record |

## Troubleshooting

- **Empty/black image**: Page not fully loaded, use wait_for_selector in navigate
- **Element not found**: Verify selector, check with extract_text first
- **Wrong area captured**: Use more specific selector

## Remember

- Call browser_navigate first
- PNG for text/UI, JPEG for photos
- No selector = full viewport
- Returns base64-encoded image
- Element screenshots capture just that element
- Vision models can analyze the base64 output
