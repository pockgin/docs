# API Contract

Pockgin serves static JSON files as its public data API. There is no runtime API server.

## Endpoints

All data files are served from the website root (e.g., `https://pockgin.github.io/pockgin/`).

### Plugin List

**Path:** `public/data/plugins.json`

Returns an array of plugin summaries for the homepage.

```json
[
  {
    "id": "example",
    "name": "Example Plugin",
    "author": "NhanAZ",
    "description": "A demo plugin for Pockgin.",
    "icon_url": "happy_ghast.png",
    "verified": false,
    "stable_version": "1.0.0",
    "total_downloads": 165,
    "stars": 30,
    "download_url": "https://github.com/pockgin/plugin-template/releases/download/v1.0.0/ExamplePlugin.phar"
  }
]
```

### Plugin Detail

**Path:** `public/data/plugins/{id}.json`

Returns full detail for a single plugin.

```json
{
  "id": "example",
  "name": "Example Plugin",
  "author": "NhanAZ",
  "description": "A demo plugin for Pockgin.",
  "repo": "https://github.com/pockgin/plugin-template",
  "archive_repo": "https://github.com/pockgin-archive/example",
  "icon_url": "happy_ghast.png",
  "verified": false,
  "approved_release_tag": "v1.0.0",
  "stars": 30,
  "total_downloads": 165,
  "last_commit_at": "2026-04-10T08:30:00Z",
  "versions": {
    "stable": {
      "version": "1.0.0",
      "tag": "v1.0.0",
      "published_at": "2026-04-01T12:00:00Z",
      "downloads": 140,
      "download_url": "https://github.com/.../ExamplePlugin.phar"
    },
    "dev": {
      "version": "1.1.0-dev",
      "tag": "v1.1.0-dev",
      "published_at": "2026-04-08T15:00:00Z",
      "downloads": 25,
      "download_url": "https://github.com/.../ExamplePlugin.phar"
    }
  },
  "recent_builds": [
    {
      "tag": "v1.0.0",
      "published_at": "2026-04-01T12:00:00Z",
      "download_url": "..."
    }
  ],
  "comments": {
    "enabled": true,
    "provider": "giscus"
  },
  "build": {
    "provider": "github-releases",
    "include_prerelease": true
  }
}
```

### Stats

**Path:** `public/data/stats.json`

Returns aggregate statistics.

```json
{
  "total_plugins": 1,
  "total_downloads": 165,
  "total_verified": 0,
  "last_sync_at": "2026-04-15T12:00:00Z"
}
```

## Registry Source Format

**Path:** `registry/plugins/{id}.json`

This is the source of truth maintained by developers and moderators. It contains only minimal fields - all enriched data (author, description, stars, downloads, versions) is generated automatically.

```json
{
  "id": "example",
  "name": "Example Plugin",
  "repo": "https://github.com/pockgin/plugin-template",
  "approved_release_tag": "v1.0.0",
  "verified": false,
  "build": {
    "provider": "github-releases",
    "include_prerelease": true
  },
  "comments": {
    "enabled": true,
    "provider": "giscus"
  }
}
```

## Stability Guarantees

- Field names and types will not change without a major version bump in the spec
- New fields may be added in a backward-compatible manner
- Removed plugins will be removed from `plugins.json` and their detail file will be deleted
- `id` values are permanent and never reused
