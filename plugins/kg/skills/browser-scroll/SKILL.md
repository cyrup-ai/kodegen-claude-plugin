---
name: browser-scroll
description: >
  Scroll web pages by pixel amount or to specific elements.
  WHEN: User needs to scroll down a page, load more content, navigate to a section, reach the footer.
  WHEN NOT: Full content already visible, user needs to click (use browser_click).
version: 0.1.0
---

# browser_scroll - Navigate Long Pages

## Core Concept

`mcp__plugin_kg_kodegen__browser_scroll` scrolls the current page. Two modes:
1. **Pixel scrolling**: Scroll by x/y amounts
2. **Element scrolling**: Scroll to bring element into view

Requires `browser_navigate` to be called first.

## Key Parameters

### Pixel Mode

#### x (optional, default: 0)
Horizontal scroll in pixels. Positive = right, negative = left.

#### y (optional, default: 0)
Vertical scroll in pixels. Positive = down, negative = up.

```json
{"y": 500}           // Scroll down 500px
{"y": -300}          // Scroll up 300px
{"x": 200, "y": 400} // Scroll right and down
```

### Element Mode

#### selector
CSS selector of element to scroll into view.

```json
{"selector": "#footer"}
{"selector": ".pricing-section"}
```

## Usage Examples

### Scroll Down
```json
browser_scroll({"y": 500})
```

### Scroll Up
```json
browser_scroll({"y": -300})
```

### Scroll to Element
```json
browser_scroll({"selector": "#contact-section"})
```

### Scroll to Footer
```json
browser_scroll({"selector": "footer"})
```

### Multiple Screen Scrolls
```json
browser_scroll({"y": 800})  // First viewport
browser_scroll({"y": 800})  // Second viewport
browser_scroll({"y": 800})  // Third viewport
```

## Response Format

```json
{
  "success": true,
  "direction": "down",
  "amount": 500,
  "message": "Scrolled by x=0, y=500"
}
```

## Common Patterns

### Infinite Scroll Content
```json
1. browser_navigate({"url": "https://example.com/feed"})
2. browser_extract_text({"selector": ".feed"})
3. browser_scroll({"y": 1000})
4. browser_extract_text({"selector": ".feed"})
// Repeat to load more content
```

### Navigate to Section
```json
1. browser_navigate({"url": "https://example.com"})
2. browser_scroll({"selector": "#pricing"})
3. browser_screenshot({})
```

### Read Long Article
```json
browser_extract_text({"selector": ".article"})
browser_scroll({"y": 800})
browser_extract_text({"selector": ".article"})
```

## When to Use What

| Goal | Method |
|------|--------|
| Load more content | Pixel scroll (y: 500-1000) |
| Go to specific section | Selector scroll |
| Return to top | Pixel scroll (y: -10000) |
| Horizontal navigation | Pixel scroll (x: N) |

## Troubleshooting

- **No scroll effect**: Page may not be scrollable, check CSS overflow
- **Element not found**: Verify selector, element may not exist
- **Wrong direction**: Positive y = down, negative y = up

## Remember

- Call browser_navigate first
- Positive y = scroll down, negative = up
- Selector mode scrolls element into view
- Amounts clamped to +/- 10000px for safety
- Combine with extract_text for infinite scroll pages
