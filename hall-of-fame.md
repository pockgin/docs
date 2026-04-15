# Hall of Fame

The Hall of Fame (`/hall-of-fame.html`) is a public recognition page that celebrates developers, contributors, and moderators who make Pockgin possible.

Its primary goals are:

- **Community motivation** – encourage healthy competition by showing public rankings.
- **Gratitude** – formally thank everyone who contributes time and code.
- **Transparency** – make it clear who maintains and moderates the registry.

---

## Sections

### Moderators

An explicit list of people who review and approve plugin submissions.

Managed via `registry/mods.json` in the `pockgin/pockgin` repository.

```json
[
  {
    "username": "NhanAZ",
    "display_name": "NhanAZ",
    "role": "Moderator"
  }
]
```

To add or remove a moderator, submit a PR editing `registry/mods.json`. No automated detection is used – the list is fully curated.

### Most Plugins

Top developers ranked by the number of plugins published on Pockgin. Derived automatically from the sync cache.
Avatar is fetched from the GitHub account that owns each plugin repository.

### Most Downloads

Top developers ranked by total download count across all their published plugins.
Avatar is fetched from the same GitHub account mapping used above.

### Top Contributors

People with the most commits across all public repositories in the `pockgin` GitHub organisation. Bot accounts are excluded.

---

## Data File

The Hall of Fame data is served as a static JSON file:

**Path:** `public/data/hall-of-fame.json`

```json
{
  "last_updated_at": "2026-05-01T03:00:00Z",
  "moderators": [
    {
      "username": "NhanAZ",
      "display_name": "NhanAZ",
      "role": "Lead Moderator",
      "avatar_url": "https://avatars.githubusercontent.com/NhanAZ?s=64"
    }
  ],
  "top_authors_by_plugins": [
    {
      "username": "NhanAZ",
      "avatar_url": "...",
      "plugin_count": 3,
      "plugins": [{ "id": "simpleeconomy", "name": "SimpleEconomy" }]
    }
  ],
  "top_authors_by_downloads": [
    {
      "username": "NhanAZ",
      "avatar_url": "...",
      "total_downloads": 1500,
      "plugin_count": 3
    }
  ],
  "top_contributors": [
    {
      "username": "NhanAZ",
      "avatar_url": "...",
      "contributions": 120,
      "repos": [
        { "repo": "pockgin", "contributions": 80 },
        { "repo": "cli", "contributions": 40 }
      ]
    }
  ]
}
```

---

## Update Schedule

The Hall of Fame sync runs automatically on the **1st of every month at 03:00 UTC** via the `Sync & Deploy` GitHub Actions workflow.

It can also be triggered manually using `workflow_dispatch` from the Actions tab.

The script responsible is `scripts/sync-hall-of-fame.js` in the `pockgin/scripts` repository.

---

## Moderator Checklist (maintaining mods.json)

- [ ] Confirm the person has agreed to take on a moderation role
- [ ] Add entry to `registry/mods.json` with correct `username` matching their GitHub handle
- [ ] Set a clear `role` label (e.g. `"Lead Moderator"`, `"Moderator"`, `"Contributor"`)
- [ ] Merge the PR – Hall of Fame will reflect the change on the next monthly sync
