# Rate Limit Strategy

## Overview

Pockgin's data pipeline relies on the GitHub API. To avoid disruption from rate limits, the system implements multiple layers of protection.

## Authentication

All GitHub API requests in CI **must** use a token:
- `GITHUB_TOKEN` (default in GitHub Actions, 5,000 requests/hr)
- A Personal Access Token (PAT) as a fallback option

Unauthenticated requests are limited to 60/hr and are not suitable for production.

## ETag Caching

Every API response is cached with its `ETag` header:
- Subsequent requests include `If-None-Match` with the cached ETag
- `304 Not Modified` responses consume **zero** quota
- Cache is stored in `.cache/` during sync runs

This dramatically reduces actual quota consumption when data hasn't changed.

## Sync Schedule

| Trigger | Frequency |
|---------|-----------|
| After PR merge to main | Immediately |
| Scheduled (cron) | Every 6–12 hours |

The schedule must never be more frequent than every 6 hours to stay safely within limits.

## Incremental Sync

When only one plugin's registry entry changes (detected by Git diff), the pipeline can:
1. Sync only the changed plugin's data
2. Regenerate only the affected `public/data/plugins/{id}.json`
3. Update `plugins.json` and `stats.json` with merged data

## Data Priority

When quota is running low (< 50 remaining), the pipeline skips non-essential data:

| Priority | Data | Action when low quota |
|----------|------|-----------------------|
| High | Plugin list, download URLs | Always fetch |
| High | Archive repo status | Always verify |
| Medium | Download counts, stars | Skip, use cached value |
| Low | plugin.yml metadata | Skip, use cached value |

## Retry & Backoff

When the API returns `403 Rate Limit` or `429 Too Many Requests`:

1. Wait 1 minute, retry
2. Wait 2 minutes, retry
3. Wait 4 minutes, retry
4. After 3 retries, mark the sync as a **soft failure** and stop

The pipeline never retries indefinitely.

## Fail-Safe Rules

1. **Never overwrite public data with empty output.** If sync fails, the last known good data is preserved.
2. **Never call GitHub API from the client browser.** The website reads only from pre-generated static JSON.
3. **If stars/downloads can't be updated**, display the old values with the `last_sync_at` timestamp.
4. **If icon resolution fails**, fall back to the default icon. Never break layout.

## Monitoring

The sync script logs:
- `remaining` rate limit after each batch of requests
- Any endpoint that was throttled
- Which plugins failed to sync and why
- Total success/failure counts

In CI, these logs are visible in the workflow run output. Failed syncs produce a warning annotation but do not delete existing public data.
