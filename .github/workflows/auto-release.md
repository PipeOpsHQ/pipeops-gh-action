# Auto-Release Management

This repository uses automated release management workflows to simplify versioning and publishing.

## Workflows

### 1. Release Management (`release.yml`)

Automatically creates GitHub releases when version tags are pushed.

**Triggers:**
- Push of semantic version tags (`v*.*.*`)
- Manual workflow dispatch

**What it does:**
- Creates a GitHub release with changelog
- Generates release notes from git commits
- Updates major version tags (`v1`, `v2`, etc.) to point to latest release
- Validates `action.yml` structure

**Usage:**

```bash
# Create and push a version tag
git tag v1.0.0
git push origin v1.0.0
# Release will be created automatically
```

### 2. Version Bump Helper (`version-bump.yml`)

Helper workflow to bump version numbers and optionally create releases.

**Triggers:**
- Manual workflow dispatch only

**What it does:**
- Bumps version (patch/minor/major)
- Creates version tag
- Optionally triggers release creation

**Usage:**

1. Go to **Actions** → **Version Bump Helper**
2. Click **Run workflow**
3. Select bump type:
   - **patch**: 1.0.0 → 1.0.1 (bug fixes)
   - **minor**: 1.0.0 → 1.1.0 (new features, backward compatible)
   - **major**: 1.0.0 → 2.0.0 (breaking changes)
4. Choose whether to create release automatically
5. Click **Run workflow**

## Release Process

### Automatic Release (Recommended)

1. **Push a version tag:**
   ```bash
   git tag v1.0.0
   git push origin v1.0.0
   ```

2. **Release workflow runs automatically:**
   - Creates GitHub release
   - Generates changelog from commits
   - Updates major version tag

### Manual Release via Workflow

1. Go to **Actions** → **Release Management**
2. Click **Run workflow**
3. Enter version (e.g., `v1.0.0`)
4. Click **Run workflow**

### Using Version Bump Helper

1. Go to **Actions** → **Version Bump Helper**
2. Select bump type and options
3. Run workflow
4. Release is created automatically if option is enabled

## Version Tag Management

### Major Version Tags

The release workflow automatically maintains major version tags (`v1`, `v2`, etc.) that point to the latest release in that major version.

**Example:**
- `v1.0.0` → creates/updates `v1` tag
- `v1.1.0` → updates `v1` tag to point to `v1.1.0`
- `v2.0.0` → creates `v2` tag, `v1` tag remains at `v1.1.0`

### User Benefits

Users can reference the action using:
- **Specific version**: `PipeOpsHQ/pipeops-action@v1.0.0` (pinned)
- **Major version**: `PipeOpsHQ/pipeops-action@v1` (auto-updates)

## Semantic Versioning

Follow [Semantic Versioning](https://semver.org/) guidelines:

- **MAJOR** (v2.0.0): Breaking changes
- **MINOR** (v1.1.0): New features, backward compatible
- **PATCH** (v1.0.1): Bug fixes, backward compatible

## Changelog Generation

The release workflow automatically generates changelogs from git commits between tags:

```
## Release v1.1.0

### Changes since v1.0.0

- Added support for custom pipelines
- Fixed authentication issue
- Improved error messages
```

## Marketplace Publishing

**Note:** Marketplace publishing still requires manual approval:

1. After automatic release is created
2. Go to **Releases** → Select the release
3. Click **Edit**
4. Check **"Publish this Action to the GitHub Marketplace"**
5. Select category and publish

## Best Practices

1. **Use semantic versioning**: Always use `v*.*.*` format
2. **Write clear commit messages**: They appear in changelog
3. **Tag from main branch**: Ensure tags are created from stable code
4. **Test before releasing**: Verify action works before tagging
5. **Update README**: Keep documentation current with new features

## Troubleshooting

### Release not created

- Check workflow run in **Actions** tab
- Verify tag format is `v*.*.*`
- Ensure workflow has `contents: write` permission

### Major version tag not updated

- Check git push permissions
- Verify tag format
- Review workflow logs for errors

### Changelog empty

- Ensure previous tag exists
- Check git history between tags
- Verify commits exist between tags
