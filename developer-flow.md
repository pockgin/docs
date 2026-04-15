# Developer Flow

Step-by-step guide for submitting a plugin to Pockgin.

## Prerequisites

- A PocketMine-MP plugin hosted on GitHub (public repo)
- An open-source license (`LICENSE` file at repo root)
- A valid `plugin.yml` with `name`, `main`, `version`, and `api` fields

## Step 1: Use the Standard Build Workflow

Clone the [plugin-template](https://github.com/pockgin/plugin-template) or add the workflow manually:

1. Copy `.github/workflows/build.yml` from the template into your repo
2. The workflow uses `@pockgin/cli` to validate and build your plugin
3. When you bump the `version` field in `plugin.yml` and push to `main`, a release is created automatically

> **Important:** Auto-release on your repo does NOT mean auto-publish on Pockgin. A moderator must still approve it.

## Step 2: Submit a Registry Entry

1. Fork the [pockgin/pockgin](https://github.com/pockgin/pockgin) repository
2. Create `registry/plugins/{your-plugin-id}.json`:

```json
{
  "id": "my-plugin",
  "name": "My Plugin",
  "repo": "https://github.com/yourname/my-plugin",
  "approved_release_tag": "v1.0.0",
  "verified": false,
  "build": {
    "provider": "github-releases",
    "include_prerelease": false
  },
  "comments": {
    "enabled": true,
    "provider": "giscus"
  }
}
```

3. Open a PR against `main`

### Naming Rules

- `id` must be **lowercase-kebab-case** (`[a-z0-9-]` only)
- `id` cannot be changed after publishing
- The filename must match: `{id}.json`
- No two plugins can share the same `id` or the same `repo` URL

## Step 3: Wait for Review

A moderator will:
- Check that your repo uses the standard Pockgin build workflow
- Verify basic compliance (no obfuscation, valid plugin.yml, license present)
- Archive your repo to `pockgin-archive`
- Merge the PR once approved

## Step 4: Update Releases

When you release a new version:
1. Bump `version` in `plugin.yml` and push
2. Open a new PR updating `approved_release_tag` in your registry entry
3. Moderator merges = new version published

## Checklist

- [ ] Public GitHub repo
- [ ] Open-source license (LICENSE file)
- [ ] Valid `plugin.yml` (name, main, version, api)
- [ ] Namespaced `main` class (e.g., `Author\Plugin`)
- [ ] Using Pockgin standard build workflow
- [ ] No obfuscated code
- [ ] No remote code execution / auto-update from untrusted sources
- [ ] Dependencies declared in `plugin.yml` if applicable
- [ ] README with basic usage description (recommended)
