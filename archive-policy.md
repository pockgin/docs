# Archive Policy

## Purpose

The `pockgin-archive` organization stores copies of approved plugins to protect against:
- Original repo being deleted
- Original repo being made private
- Force-pushes that rewrite history
- Account suspension or transfer

## Requirements

Every plugin published on Pockgin **must** have a corresponding archive before it appears on the website.

## Archive Methods

### Method 1: Fork (Preferred)

Fork the plugin repo into the `pockgin-archive` GitHub organization.

- Pros: Easy to set up, automatic relationship tracking
- Cons: Forks may lose branches if the upstream is deleted

### Method 2: Mirror

Create a standalone repo in `pockgin-archive` and push a mirror:

```bash
git clone --mirror https://github.com/developer/plugin.git
cd plugin.git
git remote set-url origin https://github.com/pockgin-archive/plugin.git
git push --mirror
```

- Pros: Fully independent copy
- Cons: Requires manual sync or a scheduled workflow

## Naming Convention

| Format | Example |
|--------|---------|
| `{id}` | `pockgin-archive/example` |
| `{owner}--{repo}` | `pockgin-archive/nhanaz--my-plugin` |

If the name is already taken, append a stable suffix: `-{short_sha}` or `-v{n}`.

## Data Tracking

The `archive_repo` URL is stored in the generated public data for each plugin:

```json
{
  "id": "example",
  "archive_repo": "https://github.com/pockgin-archive/example"
}
```

## Failure Handling

If the archive sync fails during a data pipeline run:
- The plugin's public data is **not** updated in that sync cycle
- The most recent successful public data is preserved
- The sync log reports the failure clearly

## Keeping Archives Updated

When a moderator approves a new release tag:
1. Sync the latest commits/tags to the archive repo
2. Verify the archive contains the approved tag
3. Only then merge the registry PR
