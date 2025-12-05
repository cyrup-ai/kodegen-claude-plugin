---
name: github-search-code
description: >
  Search code across all of GitHub using GitHub's code search syntax.
  WHEN: User wants to find code examples, "how do others implement X", "find usage of function Y", searching for code patterns across repositories.
  WHEN NOT: Searching local code (use fs_search), searching within a single cloned repo (use fs_search).
version: 0.1.0
---

# GitHub Search Code

## Core Concept

`mcp__plugin_kg_kodegen__github_search_code` searches code across all public GitHub repositories (and private repos you have access to) using GitHub's powerful code search syntax.

## Key Parameters

### Required
- **query** (string): Search query using GitHub code search syntax

### Optional
- **sort** (string): Sort by "indexed"
- **order** (string): "asc" or "desc"
- **page** (u32): Page number
- **per_page** (u8): Results per page (max 100)
- **enrich_stars** (boolean): Include star counts (default: false)

## GitHub Code Search Syntax

### Basic Search
- `fn main` - Search for "fn main"
- `"exact phrase"` - Exact phrase match

### Qualifiers
- `repo:owner/name` - Specific repository
- `org:organization` - Organization's repos
- `user:username` - User's repos
- `language:rust` - Filter by language
- `path:src/` - Files in path
- `filename:Cargo.toml` - Specific filename
- `extension:rs` - File extension

### Combining Qualifiers
- `language:rust path:src/ fn async` - Rust files in src/ with "fn async"
- `repo:facebook/react useState` - Search useState in React repo

## Usage Examples

### Find Rust async patterns
```json
{
  "query": "language:rust async fn",
  "per_page": 20
}
```

### Search in a specific repo
```json
{
  "query": "repo:rust-lang/rust unsafe impl",
  "per_page": 50
}
```

### Find configuration patterns with stars
```json
{
  "query": "filename:tsconfig.json strict",
  "enrich_stars": true
}
```

### Search for error handling patterns
```json
{
  "query": "language:rust Result<(), anyhow::Error>",
  "per_page": 30
}
```

## Output Fields

Each result includes:
- **name**: File name
- **path**: Full file path
- **repository_full_name**: owner/repo
- **html_url**: GitHub web URL
- **star_count**: Stars (if enrich_stars=true)

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Search all GitHub | github_search_code | Global code search |
| Search local code | fs_search | Local filesystem |
| Find repos | github_search_repositories | Repo-level search |
| Read found file | github_get_file_contents | After finding with search |

## Remember

- Searches across ALL of GitHub (public + your private repos)
- Use qualifiers to narrow down results
- Rate limited - use specific queries to get better results
- Results show file location, not content (use github_get_file_contents to read)
- `enrich_stars` adds API calls but helps find popular repos
