---
name: github-push-files
description: >
  Push multiple files to a GitHub repository in a single commit.
  WHEN: User wants to push multiple files at once, batch file updates via API, says "push these files to GitHub", "commit multiple files", needs atomic multi-file commit without local clone.
  WHEN NOT: Single file (use github_create_or_update_file for simplicity), local git workflow (use terminal git commands).
version: 0.1.0
---

# GitHub Push Files

## Core Concept

`mcp__plugin_kg_kodegen__github_push_files` pushes multiple files to a GitHub repository in a single atomic commit via the API. All files are committed together, ensuring consistency.

## Key Parameters

### Required
- **owner** (string): Repository owner
- **repo** (string): Repository name
- **branch** (string): Target branch name
- **files** (object): Map of file paths to base64-encoded content
- **message** (string): Commit message

## Usage Examples

### Push multiple source files
```json
{
  "owner": "myuser",
  "repo": "my-project",
  "branch": "main",
  "message": "Add initial project files",
  "files": {
    "src/main.rs": "Zm4gbWFpbigpIHsKICAgIHByaW50bG4hKCJIZWxsbyEiKTsKfQ==",
    "Cargo.toml": "W3BhY2thZ2VdCm5hbWUgPSAibXktcHJvamVjdCIKdmVyc2lvbiA9ICIwLjEuMCI="
  }
}
```

### Push config files to a feature branch
```json
{
  "owner": "myorg",
  "repo": "config-repo",
  "branch": "feature/new-config",
  "message": "Update configuration for v2",
  "files": {
    "config/prod.json": "eyJlbnYiOiAicHJvZCJ9",
    "config/staging.json": "eyJlbnYiOiAic3RhZ2luZyJ9"
  }
}
```

## Base64 Encoding

File contents MUST be base64-encoded. In most environments:

```bash
# Encode a file
base64 < myfile.txt

# Or in code
import base64
encoded = base64.b64encode(content.encode()).decode()
```

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Multiple files, one commit | github_push_files | Atomic batch operation |
| Single file | github_create_or_update_file | Simpler for one file |
| Local workflow | terminal (git add, commit, push) | Full git features |
| Read files first | github_get_file_contents | Get current content |

## Remember

- All files are committed ATOMICALLY in one commit
- File contents MUST be base64-encoded
- Branch must exist (create with github_create_branch if needed)
- Overwrites existing files at those paths
- Does not delete files not included in the request
- Good for CI/CD, automation, and bot operations
