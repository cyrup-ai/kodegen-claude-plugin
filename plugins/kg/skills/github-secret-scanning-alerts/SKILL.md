---
name: github-secret-scanning-alerts
description: >
  Get secret scanning alerts for exposed credentials in a repository.
  WHEN: User asks about leaked secrets, exposed credentials, API keys in code, "any secrets exposed", "check for leaked tokens".
  WHEN NOT: Code vulnerabilities (use github_code_scanning_alerts), general security analysis.
version: 0.1.0
---

# GitHub Secret Scanning Alerts

## Core Concept

`mcp__plugin_kg_kodegen__github_secret_scanning_alerts` retrieves alerts for exposed secrets (API keys, tokens, passwords) found in the repository by GitHub's secret scanning feature.

## Key Parameters

### Required
- **owner** (string): Repository owner
- **repo** (string): Repository name

### Optional
- **state** (string): Filter by state - "open" or "resolved"
- **secret_type** (string): Filter by secret type (e.g., "github_token", "aws_access_key_id")
- **resolution** (string): Filter by resolution - "false_positive", "wont_fix", "revoked", "used_in_tests"

## Usage Examples

### Get all open secret alerts
```json
{
  "owner": "myorg",
  "repo": "my-project",
  "state": "open"
}
```

### Find AWS credential leaks
```json
{
  "owner": "myorg",
  "repo": "my-project",
  "secret_type": "aws_access_key_id"
}
```

### Get resolved false positives
```json
{
  "owner": "myorg",
  "repo": "my-project",
  "state": "resolved",
  "resolution": "false_positive"
}
```

## Output Fields

Each alert includes:
- **number**: Alert ID
- **state**: open or resolved
- **secret_type**: Type of secret detected
- **resolution**: How it was resolved (if resolved)
- **created_at**: When discovered
- **html_url**: GitHub URL to view alert

## Common Secret Types

- `github_token` - GitHub personal access tokens
- `aws_access_key_id` - AWS access keys
- `aws_secret_access_key` - AWS secret keys
- `slack_webhook_url` - Slack webhooks
- `stripe_api_key` - Stripe API keys
- `google_api_key` - Google API keys
- And 100+ more provider patterns

## Resolution Types

- **false_positive**: Not actually a secret
- **wont_fix**: Accepted risk
- **revoked**: Secret was rotated/revoked
- **used_in_tests**: Test data, not real credentials

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Exposed secrets | github_secret_scanning_alerts | Credential detection |
| Code vulnerabilities | github_code_scanning_alerts | SAST/code analysis |
| Security audit | Both tools | Complete review |

## Remember

- Requires GitHub Advanced Security or public repo
- Alerts immediately when secrets are detected
- REVOKE exposed secrets immediately - they're compromised
- GitHub also notifies the secret provider (e.g., AWS) automatically
- Secret scanning works on all branches and history
- Cannot scan encrypted or binary files
