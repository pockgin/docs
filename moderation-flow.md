# Moderation Flow

Guide for moderators reviewing plugin submissions on Pockgin.

## Core Principle

**Merge PR = Publish the approved tag.**

Only the release tag specified in `approved_release_tag` will appear on the public website. No release is promoted automatically.

## Review Process

### 1. Validate Registry Entry

Ensure the PR modifies `registry/plugins/{id}.json` and:
- `id` matches the filename
- `id` is lowercase-kebab-case (`[a-z0-9-]`)
- `repo` is a valid public GitHub repo URL
- `approved_release_tag` points to an existing GitHub Release
- `build.provider` is `"github-releases"`
- No duplicate `id` or `repo` in existing registry

### 2. Check Build Compliance

Visit the plugin repo and verify:
- The repo uses `.github/workflows/build.yml` from `pockgin/cli`
- CI builds are passing
- The release artifact (`.phar`) was built by the standard workflow
- `build-meta.json` is present in the release

### 3. Check Plugin Quality (Lite Rules)

**MUST (reject if violated):**
- No obfuscated source code
- Valid `plugin.yml` with `name`, `main`, `version`, `api`
- Open-source license file present
- Dependencies declared in `plugin.yml`
- No remote code execution or auto-update from untrusted sources
- Namespaced `main` class (recommended: `Author\Plugin`)

**SHOULD (can approve if safe):**
- README with clear description
- Changelog in releases
- No excessive logging at startup/shutdown
- No heavy/blocking operations on main thread

### 4. Archive the Plugin

Before merging, ensure the plugin is archived:
- Fork the plugin repo into the `pockgin-archive` organization, OR
- Mirror/sync to a dedicated archive repo
- Verify the archive is accessible

### 5. Merge or Request Changes

- If everything checks out: **merge the PR**
- If issues are found: leave clear, specific feedback with suggestions for fixing
- Be constructive - help developers improve rather than gatekeep

### Featured Label Policy

Moderators may mark selected plugins as `featured` in registry metadata.

- `featured: true` means the plugin is highlighted as editor-recommended
- Featured plugins are sorted above non-featured plugins in the public list
- Featured status does **not** replace security/compliance review requirements
- Featured can be granted or removed at any time by moderators

## Updating Approved Releases

When a developer submits a PR to update `approved_release_tag`:
1. Verify the new tag exists and was built by the standard workflow
2. Check the release for any new compliance issues
3. Ensure the archive is up to date
4. Merge the PR

## Moderator Checklist

- [ ] Registry JSON is valid schema
- [ ] No duplicate ID or repo URL
- [ ] Plugin repo uses Pockgin build workflow
- [ ] CI is passing
- [ ] `approved_release_tag` exists as a GitHub Release
- [ ] Plugin passes MUST rules
- [ ] Archive exists at `pockgin-archive`
- [ ] Merge PR to publish
