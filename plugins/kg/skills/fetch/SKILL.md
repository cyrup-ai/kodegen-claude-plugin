---
name: fetch
description: >
  Fetch a single web page with ANSI-highlighted markdown display.
  WHEN: User wants to "fetch a page", "get page content", "view URL", "read documentation page".
  WHEN NOT: Multi-page crawling (use scrape_url), web search (use web_search).
version: 0.1.0
---

# fetch - Single Page Fetcher

## Core Concept

`mcp__plugin_kg_kodegen__fetch` fetches a single web page and returns ANSI syntax-highlighted markdown for beautiful terminal display. Simplified wrapper around `scrape_url`.

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `url` | string | Yes | URL to fetch |

## Usage

```json
{
  "url": "https://docs.rs/tokio/latest/tokio/"
}
```

## Output

| Field | Type | Description |
|-------|------|-------------|
| `display` | string | ANSI-highlighted markdown (for terminal) |
| `path` | string | Absolute path to saved .md file |
| `search_helper` | string | TypeScript snippet for follow-up search |
| `url` | string | URL that was fetched |
| `title` | string? | Page title if available |
| `content_length` | number | Content size in bytes |

## Example Response

```json
{
  "display": "\u001b[38;2;...# Tokio\n\nA runtime for async Rust...\u001b[0m",
  "path": "/project/.kodegen/citescrape/docs.rs/tokio/index.md",
  "search_helper": "scrape_url({ action: 'SEARCH', crawl_id: 0, query: '<your query>' })",
  "url": "https://docs.rs/tokio/latest/tokio/",
  "title": "Tokio",
  "content_length": 12345
}
```

## Workflow

1. Call `fetch` with target URL
2. View ANSI-highlighted content in terminal
3. Use `path` to read raw markdown if needed
4. Use `search_helper` for follow-up queries on the content

## Remember

- **Single page only** - use `scrape_url` for multi-page crawling
- **ANSI output** - `display` contains terminal escape codes for colors
- **Search ready** - content is indexed for follow-up `scrape_url` SEARCH queries
- **File persisted** - markdown saved to disk at `path`
