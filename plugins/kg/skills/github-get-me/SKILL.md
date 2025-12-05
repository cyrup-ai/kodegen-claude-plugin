---
name: github-get-me
description: >
  Get authenticated GitHub user information.
  WHEN: User asks "who am I on GitHub", "what's my GitHub username", needs to check authentication status, wants their profile info, asks "am I logged in".
  WHEN NOT: Searching for other users (use github_search_users), getting info about a specific user.
version: 0.1.0
---

# GitHub Get Me

## Core Concept

`mcp__plugin_kg_kodegen__github_get_me` retrieves information about the currently authenticated GitHub user. This is useful for verifying authentication and getting your username for other operations.

## Key Parameters

No parameters required - uses GITHUB_TOKEN for authentication.

## Usage Examples

### Get current user info
```json
{}
```

## Output Fields

- **login**: GitHub username
- **id**: Numeric user ID
- **name**: Display name
- **email**: Primary email
- **avatar_url**: Profile picture URL
- **html_url**: Profile page URL
- **bio**: User bio
- **location**: Location
- **company**: Company
- **followers**: Follower count
- **following**: Following count
- **public_repos**: Number of public repositories
- **created_at**: Account creation date

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Get own info | github_get_me | Returns authenticated user |
| Search users | github_search_users | Find other users |
| Get repo owner | Use owner from repo operations | Part of repo info |

## Remember

- Requires GITHUB_TOKEN environment variable
- Returns empty/error if token is invalid
- Useful to verify authentication before other operations
- Use the `login` field as `owner` for your own repos
