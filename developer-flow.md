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

## Step 1b: Declare Library Dependencies (if any)

If your plugin depends on external libraries (virions, shared code), declare them in `pockgin.libs.yml` at the repo root:

```yaml
libs:
  - id: simplesql
    repo: NhanAZ-Libraries/SimpleSQL
    version: ^1.0.0
    target: src/NhanAZ/SimpleSQL
    src_path: src/NhanAZ/SimpleSQL

  - id: libasynql
    repo: poggit/libasynql
    version: ^4.0.0
    target: src/poggit/libasynql
    src_path: libasynql/src/poggit/libasynql
```

### Fields

| Field | Required | Description |
|-------|----------|-------------|
| `id` | Yes | Short identifier for the lib |
| `repo` | Yes | GitHub `owner/repo` |
| `version` | No | Semver range (e.g., `^1.0.0`). Resolves to the best matching tag. |
| `ref` | No | Explicit tag, branch, or commit SHA. Overrides `version`. |
| `target` | Yes | Where to inject the source (relative to plugin root) |
| `src_path` | No | Path inside the lib repo to copy from. Auto-detected if omitted. |

### How it works

1. `pockgin build` automatically detects `pockgin.libs.yml`
2. Resolves versions to exact tags or commits
3. Writes `pockgin.libs.lock.yml` (commit this file!)
4. Clones and injects lib source into your `src/` before building the `.phar`

### Version resolution rules

- If the lib repo has tags matching semver, the best match is selected.
- If no matching tag exists, the default branch HEAD commit is used.
- **Branch names (`main`, `master`) are never used in production builds.** Only tags or commit SHAs.
- `build-meta.json` records exactly which lib versions were injected.

### Manual commands (optional)

```bash
pockgin libs resolve .    # Resolve versions -> write lock file
pockgin libs vendor .     # Clone and inject libs from lock file
```

> **Tip:** You don't need to run these manually. `pockgin build` handles everything automatically.

## Step 2: Submit a Registry Entry

1. Fork the [pockgin/pockgin](https://github.com/pockgin/pockgin) repository
2. Create `registry/plugins/{your-plugin-id}.json`:

```json
{
  "id": "my-plugin",
  "name": "My Plugin",
  "repo": "https://github.com/yourname/my-plugin",
  "approved_release_tag": "v1.0.0",
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
