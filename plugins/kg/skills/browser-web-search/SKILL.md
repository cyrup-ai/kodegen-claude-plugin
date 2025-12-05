---
name: browser-web-search
description: >
  Search the web using DuckDuckGo and return structured results.
  WHEN: User asks to search the web, find information online, look up something, research a query, discover relevant links.
  WHEN NOT: User has a specific URL to visit (use browser_navigate), needs comprehensive multi-page research (use browser_research).
version: 0.1.0
---

# browser_web_search - Web Search

## Core Concept

`mcp__plugin_kg_kodegen__browser_web_search` performs web searches using DuckDuckGo and returns structured results with titles, URLs, and snippets. Returns up to 10 results ranked by relevance.

Uses DuckDuckGo to avoid CAPTCHA issues. First search takes ~5-6s (browser launch), subsequent searches take ~3-4s.

## Key Parameters

### query (required)
The search query string.

```json
{"query": "rust async programming"}
{"query": "how to use tokio runtime"}
```

## Usage Examples

### Basic Search
```json
browser_web_search({"query": "rust async programming"})
```

### Technical Documentation Search
```json
browser_web_search({"query": "tokio::spawn documentation"})
```

### Error Message Search
```json
browser_web_search({"query": "rust borrow checker error E0382"})
```

## Response Format

```json
{
  "success": true,
  "query": "rust async programming",
  "results_count": 10,
  "results": [
    {
      "rank": 1,
      "title": "Async Programming in Rust",
      "url": "https://rust-lang.org/async",
      "snippet": "Learn about async/await in Rust..."
    }
  ]
}
```

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Quick web lookup | browser_web_search | Fast, structured results |
| Visit specific URL | browser_navigate | Direct page access |
| Deep topic research | browser_research | Multi-page with AI summary |
| Find code examples | browser_web_search | Search then navigate to results |

## Common Patterns

### Search Then Navigate
```json
1. browser_web_search({"query": "rust serde tutorial"})
2. browser_navigate({"url": "<first result URL>"})
3. browser_extract_text({})
```

### Research Workflow
```json
1. browser_web_search({"query": "topic overview"})
2. Review results, select relevant URLs
3. browser_navigate to each, extract content
```

## Remember

- Returns up to 10 structured results
- Uses DuckDuckGo (avoids CAPTCHA)
- First search slower due to browser launch
- Results include rank, title, URL, snippet
- Use for discovery, then navigate to specific pages
