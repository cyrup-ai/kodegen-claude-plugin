---
name: github-search-repositories
description: >
  Search for repositories on GitHub.
  WHEN: User wants to find projects, "find repos about X", "popular Rust libraries", searching for repositories by topic, language, or keywords.
  WHEN NOT: Searching code content (use github_search_code), searching for users (use github_search_users).
version: 0.1.0
---

# GitHub Search Repositories

## Core Concept

`mcp__plugin_kg_kodegen__github_search_repositories` searches for repositories on GitHub using GitHub's repository search syntax. Find projects by topic, language, stars, and more.

## Key Parameters

### Required
- **query** (string): Search query using GitHub repo search syntax

### Optional
- **sort** (string): "stars", "forks", or "updated"
- **order** (string): "asc" or "desc"
- **page** (u32): Page number
- **per_page** (u8): Results per page (max 100)

## GitHub Repo Search Syntax

### Basic Search
- `cli tool` - Repos with "cli" and "tool"
- `"web framework"` - Exact phrase

### Qualifiers
- `language:rust` - Filter by language
- `stars:>1000` - More than 1000 stars
- `stars:100..500` - Stars in range
- `forks:>100` - Fork count
- `topic:machine-learning` - By topic
- `user:username` - User's repos
- `org:organization` - Org's repos
- `created:>2024-01-01` - Created after date
- `pushed:>2024-01-01` - Recently pushed
- `archived:false` - Not archived
- `is:public` or `is:private` - Visibility

### Combining
- `language:rust stars:>100 cli` - Popular Rust CLI tools
- `topic:api language:python stars:>500` - Popular Python API projects

## Usage Examples

### Find popular Rust CLI tools
```json
{
  "query": "language:rust topic:cli stars:>100",
  "sort": "stars",
  "order": "desc"
}
```

### Find recent machine learning projects
```json
{
  "query": "topic:machine-learning pushed:>2024-01-01",
  "sort": "updated",
  "per_page": 20
}
```

### Find active web frameworks
```json
{
  "query": "topic:web-framework stars:>1000 archived:false",
  "sort": "stars",
  "order": "desc"
}
```

## Output Fields

Each repository includes:
- **full_name**: owner/repo
- **description**: Repo description
- **html_url**: GitHub URL
- **language**: Primary language
- **stars/forks/watchers**: Popularity metrics
- **topics**: Topic tags
- **created_at/updated_at/pushed_at**: Dates
- **archived/fork**: Status flags

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Find repositories | github_search_repositories | Repo-level search |
| Find code patterns | github_search_code | Code content search |
| Find users/orgs | github_search_users | User search |
| Get repo details | After search, use repo tools | Full repo info |

## Remember

- Use qualifiers to narrow results
- Sort by stars for popular repos, updated for active ones
- Topics help find specialized projects
- Check archived status for maintained projects
- Stars/forks indicate community adoption
