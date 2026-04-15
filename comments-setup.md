# Comments Setup (Global Giscus)

This project supports a global fallback comments configuration so plugin owners do not need to configure Giscus per plugin.

## Repositories

- Discussion repo: [pockgin/pockgin-comments](https://github.com/pockgin/pockgin-comments)
- Main site repo: `pockgin/pockgin`

## One-time setup

1. In `pockgin/pockgin-comments`, enable **GitHub Discussions**.
2. Create a category (recommended: `Plugin Comments`).
3. Open [giscus.app](https://giscus.app) and select:
   - repo: `pockgin/pockgin-comments`
   - your discussion category
4. Copy:
   - `repo_id`
   - `category_id`
5. Edit `pockgin/comments-config.js`:

```js
window.POCKGIN_GISCUS_DEFAULT = {
  repo: "pockgin/pockgin-comments",
  repo_id: "R_kgDOxxxxxx",
  category: "Plugin Comments",
  category_id: "DIC_kwDOxxxxxx4Cyyyy",
  mapping: "specific",
};
```

## Mapping strategy

- Mapping uses `specific`.
- Thread term is generated automatically as:
  - `plugin:{plugin-id}`
- Example terms:
  - `plugin:simpleeconomy`
  - `plugin:uncensor`

This guarantees one stable discussion thread per plugin.

## Plugin behavior

- If `comments.enabled=true` and `comments.provider="giscus"`:
  - First tries `comments.giscus` inside plugin data.
  - If missing, falls back to `window.POCKGIN_GISCUS_DEFAULT`.
- If fallback config is incomplete, UI shows an explicit setup warning.
