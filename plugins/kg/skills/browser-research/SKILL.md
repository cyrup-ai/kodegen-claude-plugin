---
name: browser-research
description: >
  Deep web research with AI-powered summarization across multiple pages.
  WHEN: User asks to research a topic, needs comprehensive information, wants a summary of web findings, asks "research X for me".
  WHEN NOT: Quick single-page lookup (use browser_navigate + browser_extract_text), simple web search (use browser_web_search).
version: 0.1.0
---

# browser_research - Deep Web Research

## Core Concept

`mcp__plugin_kg_kodegen__browser_research` performs comprehensive web research by:
1. Searching the web for your query
2. Visiting multiple pages (configurable depth)
3. Extracting and analyzing content
4. Generating an AI-powered summary with sources

Uses session-based execution with background continuation. Actions: RESEARCH, READ, LIST, KILL.

## Key Parameters

### action (required)
- `RESEARCH`: Start new research
- `READ`: Check progress of active session
- `LIST`: Show all active sessions
- `KILL`: Terminate a session

### query (required for RESEARCH)
The research topic.

### session (optional, default: 0)
Session slot number for parallel research.

### max_pages (optional, default: 5)
Number of pages to analyze (3-15).

### max_depth (optional, default: 2)
Link-following depth (1-4).

### search_engine (optional, default: "google")
Options: "google", "bing", "duckduckgo"

### await_completion_ms (optional, default: 300000)
Timeout in milliseconds. 0 = fire-and-forget background task.

## Usage Examples

### Start Research
```json
browser_research({
  "action": "RESEARCH",
  "query": "Rust async patterns best practices",
  "max_pages": 5,
  "session": 0
})
```

### Deep Research
```json
browser_research({
  "action": "RESEARCH",
  "query": "WebAssembly performance optimization",
  "max_pages": 10,
  "max_depth": 3,
  "await_completion_ms": 600000
})
```

### Check Progress
```json
browser_research({
  "action": "READ",
  "session": 0
})
```

### Fire-and-Forget
```json
browser_research({
  "action": "RESEARCH",
  "query": "topic",
  "await_completion_ms": 0
})
// Returns immediately, research continues in background
// Use READ to check progress later
```

### Kill Session
```json
browser_research({
  "action": "KILL",
  "session": 0
})
```

## Response Format

```json
{
  "session": 0,
  "status": "completed",
  "query": "Rust async patterns",
  "pages_analyzed": 5,
  "completed": true,
  "summary": "AI-generated summary of findings...",
  "sources": [
    {"url": "...", "title": "...", "summary": "..."}
  ]
}
```

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Deep topic research | browser_research | Multi-page AI summary |
| Quick web search | browser_web_search | Fast, simple results |
| Single page content | browser_navigate + extract_text | Direct access |
| Complex automation | browser_agent | Multi-step workflows |

## Remember

- Research runs in background (check with READ)
- Increase max_pages for broader coverage
- Use max_depth for following links
- Session slots enable parallel research
- Timeout returns partial results, continues in background
- KILL cleans up completed sessions
