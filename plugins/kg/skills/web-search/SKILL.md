---
name: web-search
description: >
  Perform DuckDuckGo web searches and return structured results.
  WHEN: User wants to "search the web", "find information online", "look up", "research topic".
  WHEN NOT: Know exact URL (use browser_navigate), crawling sites (use scrape_url).
version: 0.1.0
---

# web_search - DuckDuckGo Web Search

## Core Concept

`mcp__plugin_kg_kodegen__web_search` performs web searches using DuckDuckGo and returns structured results with titles, URLs, and snippets. Uses headless browser with stealth configuration to avoid bot detection.

## Key Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `query` | string | Yes | Search query |

## Usage Example

```json
{ "query": "rust async programming tutorial" }
```

## Response Format

```json
{
  "success": true,
  "query": "rust async programming tutorial",
  "results_count": 10,
  "results": [
    {
      "rank": 1,
      "title": "Async Programming in Rust - The Rust Book",
      "url": "https://doc.rust-lang.org/book/ch16-00-concurrency.html",
      "snippet": "Learn about async/await syntax and futures in Rust..."
    },
    {
      "rank": 2,
      "title": "Tokio Tutorial",
      "url": "https://tokio.rs/tokio/tutorial",
      "snippet": "Build reliable network applications with Tokio..."
    }
  ]
}
```

## Result Fields

| Field | Description |
|-------|-------------|
| `rank` | Result position (1-10) |
| `title` | Page title |
| `url` | Page URL |
| `snippet` | Description excerpt |

## Performance Notes

- **First search**: ~5-6 seconds (browser launch)
- **Subsequent searches**: ~3-4 seconds
- **Max results**: 10 per query
- Uses DuckDuckGo to avoid CAPTCHA issues

## Use Cases

1. **Research topics** - Find documentation and tutorials
2. **Discover libraries** - Search for packages and tools
3. **Find solutions** - Look up error messages and fixes
4. **Gather information** - Research before code generation

## Workflow Examples

### Research Before Coding
```json
{ "query": "rust error handling best practices" }
```

### Find Library Documentation
```json
{ "query": "serde json serialization rust" }
```

### Debug Error Messages
```json
{ "query": "rust borrow checker error E0382" }
```

## Remember

- Returns up to 10 results per search
- Results include title, URL, and snippet
- First search is slower (browser initialization)
- Uses DuckDuckGo - good for avoiding CAPTCHAs
- For known URLs, use browser tools instead
- For site-specific search, use scrape_url with SEARCH action
