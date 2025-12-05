---
name: browser-extract-text
description: >
  Extract text content from web pages or specific elements.
  WHEN: User wants to read page content, get article text, extract data from a page, see what's on a page.
  WHEN NOT: User wants to see visual appearance (use browser_screenshot), user needs to interact (use browser_click or browser_type_text).
version: 0.1.0
---

# browser_extract_text - Get Page Content

## Core Concept

`mcp__plugin_kg_kodegen__browser_extract_text` extracts visible text content from the current page. Supports:
- Full page extraction (no selector)
- Specific element extraction (with CSS selector)
- Automatic SPA handling with HTML fallback

Requires `browser_navigate` to be called first.

## Key Parameters

### selector (optional)
CSS selector to extract text from specific element. If omitted, extracts full page.

```json
{}  // Full page
{"selector": "#content"}  // Specific element
{"selector": "article.post"}  // By class
```

## Usage Examples

### Full Page Text
```json
browser_extract_text({})
```

### Specific Element
```json
browser_extract_text({"selector": "#main-content"})
```

### Article Content
```json
browser_extract_text({"selector": "article"})
```

### Navigation Menu
```json
browser_extract_text({"selector": "nav.main-menu"})
```

### Table Data
```json
browser_extract_text({"selector": "table.data-table"})
```

## Response Format

```json
{
  "success": true,
  "text": "Extracted text content...",
  "length": 1234
}
```

## Common Patterns

### Read Page Content
```json
1. browser_navigate({"url": "https://example.com/article"})
2. browser_extract_text({"selector": "article"})
```

### Extract After Interaction
```json
1. browser_navigate({"url": "https://example.com/search"})
2. browser_type_text({"selector": "#search", "text": "query"})
3. browser_click({"selector": "#search-btn"})
4. browser_extract_text({"selector": ".results"})
```

### Extract Multiple Sections
```json
browser_extract_text({"selector": ".header"})
browser_extract_text({"selector": ".content"})
browser_extract_text({"selector": ".footer"})
```

## Selector Guide

| Target | Selector |
|--------|----------|
| Main content | `main`, `#content`, `.content` |
| Article | `article`, `.post`, `.article` |
| Navigation | `nav`, `.nav`, `.menu` |
| Footer | `footer`, `.footer` |
| Specific ID | `#unique-id` |
| By class | `.class-name` |

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Read text content | browser_extract_text | Structured text output |
| Visual verification | browser_screenshot | See actual appearance |
| Check element exists | browser_extract_text with selector | Returns error if not found |
| Parse tables | browser_extract_text | Extracts table as text |

## Troubleshooting

- **Empty text**: Page may be SPA still loading, add wait_for_selector to navigate
- **Element not found**: Verify selector syntax, may be in iframe (not supported)
- **Wrong content**: Use more specific selector

## Remember

- Call browser_navigate first
- No selector = full page text
- Handles SPAs with automatic HTML fallback
- Returns character count in response
- First matching element used when selector matches multiple
