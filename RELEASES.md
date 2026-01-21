# Automated Release Management

This repository uses automated workflows to manage releases, version tags, and changelogs.

## Quick Start

### Option 1: Push a Version Tag (Recommended)

```bash
# Create and push a version tag
git tag v1.0.0
git push origin v1.0.0
```

The release workflow will automatically:
- ✅ Create a GitHub release
- ✅ Generate changelog from commits
- ✅ Update major version tag (`v1`)

### Option 2: Use Version Bump Helper

1. Go to **Actions** → **Version Bump Helper**
2. Select bump type (patch/minor/major)
3. Enable "Create release after bumping version"
4. Run workflow

## Workflows

### `release.yml` - Automatic Release Creation

**Triggers:**
- Push of version tags (`v*.*.*`)
- Manual workflow dispatch

**Features:**
- Creates GitHub release with auto-generated changelog
- Updates major version tags (`v1`, `v2`, etc.)
- Validates action.yml structure
- Generates release notes from git commits

### `version-bump.yml` - Version Bump Helper

**Triggers:**
- Manual workflow dispatch only

**Features:**
- Automatically bumps version (patch/minor/major)
- Creates version tag
- Optionally triggers release creation

## Version Tag Strategy

### Major Version Tags

The workflow maintains major version tags that point to the latest release:

- `v1.0.0` → creates/updates `v1` tag
- `v1.1.0` → updates `v1` tag to `v1.1.0`
- `v2.0.0` → creates `v2` tag, `v1` stays at `v1.1.0`

### User Benefits

Users can reference the action using:
- **Pinned**: `PipeOpsHQ/pipeops-action@v1.0.0`
- **Auto-update**: `PipeOpsHQ/pipeops-action@v1`

## Examples

### Create a Patch Release (Bug Fix)

```bash
git tag v1.0.1
git push origin v1.0.1
```

### Create a Minor Release (New Feature)

```bash
git tag v1.1.0
git push origin v1.1.0
```

### Create a Major Release (Breaking Change)

```bash
git tag v2.0.0
git push origin v2.0.0
```

### Use Version Bump Helper

1. Go to Actions → Version Bump Helper
2. Select "minor" for new features
3. Enable "Create release after bumping version"
4. Click "Run workflow"

## Changelog Generation

Changelogs are automatically generated from git commits between tags:

```
## 📦 Release v1.1.0

### Changes since v1.0.0

- Added support for custom pipelines
- Fixed authentication token handling
- Improved error messages
- Updated documentation
```

## Marketplace Publishing

**Note:** Marketplace publishing requires manual approval:

1. After automatic release is created
2. Go to **Releases** → Select the release
3. Click **Edit**
4. Check **"Publish this Action to the GitHub Marketplace"**
5. Select category and publish

## Best Practices

1. ✅ Use semantic versioning (`v*.*.*`)
2. ✅ Write clear commit messages (they appear in changelog)
3. ✅ Tag from main/master branch
4. ✅ Test before releasing
5. ✅ Update README with new features

## Troubleshooting

### Release not created?

- Check Actions tab for workflow run
- Verify tag format is `v*.*.*`
- Ensure workflow has `contents: write` permission

### Major version tag not updated?

- Check git push permissions
- Review workflow logs
- Verify tag format

### Empty changelog?

- Ensure previous tag exists
- Check git history between tags
- Verify commits exist

## See Also

- [Auto-Release Documentation](.github/workflows/auto-release.md) - Detailed workflow documentation
- [Publishing Guide](PUBLISHING.md) - Marketplace publishing instructions
