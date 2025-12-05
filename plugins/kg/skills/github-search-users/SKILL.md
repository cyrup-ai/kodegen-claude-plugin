---
name: github-search-users
description: >
  Search for GitHub users and organizations.
  WHEN: User wants to find developers, organizations, "who maintains X", "find contributors", searching for people or orgs on GitHub.
  WHEN NOT: Getting own info (use github_get_me), searching repos (use github_search_repositories).
version: 0.1.0
---

# GitHub Search Users

## Core Concept

`mcp__plugin_kg_kodegen__github_search_users` searches for GitHub users and organizations using GitHub's user search syntax.

## Key Parameters

### Required
- **query** (string): Search query using GitHub user search syntax

### Optional
- **sort** (string): "followers", "repositories", or "joined"
- **order** (string): "asc" or "desc"
- **page** (u32): Page number
- **per_page** (u8): Results per page (max 100)

## GitHub User Search Syntax

### Basic Search
- `john` - Users with "john" in username/name
- `"Jane Doe"` - Exact name match

### Qualifiers
- `type:user` - Only users (not orgs)
- `type:org` - Only organizations
- `location:California` - By location
- `language:rust` - Primary language
- `followers:>1000` - Follower count
- `repos:>50` - Repository count
- `created:>2020-01-01` - Account created after

### Combining
- `language:rust followers:>100 type:user` - Popular Rust developers
- `type:org location:Germany` - German organizations

## Usage Examples

### Find Rust developers with many followers
```json
{
  "query": "language:rust followers:>500 type:user",
  "sort": "followers",
  "order": "desc"
}
```

### Find organizations in tech hubs
```json
{
  "query": "type:org location:\"San Francisco\"",
  "sort": "repositories",
  "per_page": 20
}
```

### Find active open source contributors
```json
{
  "query": "repos:>100 type:user",
  "sort": "repositories",
  "order": "desc"
}
```

## Output Fields

Each user/org includes:
- **login**: Username
- **id**: Numeric ID
- **avatar_url**: Profile picture
- **html_url**: Profile URL
- **user_type**: "User" or "Organization"
- **name/bio/location**: Profile info (if available)
- **followers**: Follower count (if available)

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Find users/orgs | github_search_users | User search |
| Get own info | github_get_me | Authenticated user |
| Find repos | github_search_repositories | Repo search |
| Find code | github_search_code | Code search |

## Remember

- Returns both users and organizations by default
- Use `type:user` or `type:org` to filter
- Some profile fields may be null (privacy settings)
- Location is free-text, not standardized
- Language filter based on their repository languages
