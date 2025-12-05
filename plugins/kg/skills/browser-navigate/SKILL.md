---
name: browser-navigate
description: >
  Navigate browser to a URL. Entry point for all browser automation workflows.
  WHEN: User wants to visit a webpage, open a URL, load a website, go to a page, access a site.
  WHEN NOT: User wants to search the web (use browser_web_search), needs deep research on a topic (use browser_research).
version: 0.1.0
---

# browser_navigate - Load Web Pages

## Core Concept

`mcp__plugin_kg_kodegen__browser_navigate` is the **entry point** for all browser automation. It loads a URL in a headless Chromium browser and waits for the page to fully load.

You MUST call `browser_navigate` before using any other browser tool (click, type, screenshot, extract_text, scroll).

## Key Parameters

### url (required)
The URL to navigate to. Must start with `http://` or `https://`.

```json
{"url": "https://www.rust-lang.org"}
{"url": "http://localhost:3000"}
```

### timeout_ms (optional, default: 30000)
Maximum time to wait for page load in milliseconds.

```json
{"url": "https://slow-site.com", "timeout_ms": 60000}
```

### wait_for_selector (optional)
CSS selector to wait for before returning. Essential for SPAs.

```json
{"url": "https://spa-app.com", "wait_for_selector": ".app-loaded"}
{"url": "https://example.com", "wait_for_selector": "#content"}
```

## Usage Examples

### Basic Navigation
```json
browser_navigate({"url": "https://www.example.com"})
```

### Wait for SPA Content
```json
browser_navigate({
  "url": "https://react-app.com",
  "wait_for_selector": ".main-content",
  "timeout_ms": 45000
})
```

### Local Development
```json
browser_navigate({"url": "http://localhost:8080"})
```

## Response Format

```json
{
  "success": true,
  "url": "https://www.example.com",
  "title": "Example Domain",
  "status_code": 200
}
```

The `url` field contains the **final URL** after any redirects.

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Visit a specific URL | browser_navigate | Direct page access |
| Search the web | browser_web_search | Returns structured results |
| Research a topic | browser_research | Multi-page deep research with AI summary |
| Read URL content only | fs_read_file with URL | Simpler, no browser needed |

## Common Errors

- **"URL must start with http:// or https://"**: Add protocol prefix
- **"Navigation timeout"**: Increase timeout_ms or verify URL is accessible
- **"Selector not found"**: Check CSS selector syntax, increase timeout

## Remember

- Always call navigate FIRST before other browser tools
- Use `wait_for_selector` for JavaScript-heavy sites
- The page persists in memory for subsequent tool calls
- Redirects are followed automatically
- Check `url` in response to verify final destination
