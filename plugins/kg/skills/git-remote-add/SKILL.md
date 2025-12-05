---
name: git-remote-add
description: >
  Add a new remote repository to Git configuration.
  WHEN: User says "add remote", "add upstream", "configure remote", "set up remote", "add origin".
  WHEN NOT: Remote already exists (check with git_remote_list first), removing remote (use git_remote_remove).
version: 0.1.0
---

# Git Remote Add

## Core Concept

`mcp__plugin_kg_kodegen__git_remote_add` configures a named remote repository. Remotes enable pushing, pulling, and fetching from external repositories.

## Key Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| path | string | Yes | - | Path to the Git repository |
| name | string | Yes | - | Remote name (e.g., "origin", "upstream") |
| url | string | Yes | - | Remote URL (HTTPS, SSH, or file://) |
| force | boolean | No | false | Overwrite if remote name exists |

## Usage Examples

### Add origin remote (HTTPS)
```json
{
  "path": ".",
  "name": "origin",
  "url": "https://github.com/user/repo.git"
}
```

### Add upstream for fork workflow (SSH)
```json
{
  "path": ".",
  "name": "upstream",
  "url": "git@github.com:original-owner/repo.git"
}
```

### Update existing remote URL
```json
{
  "path": "/project",
  "name": "origin",
  "url": "https://github.com/new-org/repo.git",
  "force": true
}
```

### Add backup remote
```json
{
  "path": ".",
  "name": "backup",
  "url": "file:///mnt/backup/repo.git"
}
```

## URL Formats

| Format | Example | Use Case |
|--------|---------|----------|
| HTTPS | `https://github.com/user/repo.git` | Public repos, CI/CD |
| SSH | `git@github.com:user/repo.git` | Authenticated access |
| File | `file:///path/to/repo.git` | Local backups |

## Remote Naming Conventions

- `origin` - Your primary repository (default for clone)
- `upstream` - Original repo when working on a fork
- `backup` - Backup location
- `staging` - Staging environment

## When to Use What

| Goal | Tool | Why |
|------|------|-----|
| Add new remote | git_remote_add | Configures remote |
| List remotes | git_remote_list | See existing remotes |
| Remove remote | git_remote_remove | Deletes remote config |
| Change URL | git_remote_add with force | Overwrites existing |

## Remember

- Check existing remotes with git_remote_list before adding
- Use `force: true` to update URL of existing remote
- HTTPS requires authentication per operation
- SSH uses key-based authentication (set up separately)
- "origin" is convention for primary remote
