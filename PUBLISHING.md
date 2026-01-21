# Publishing to GitHub Marketplace

This guide walks you through publishing the PipeOps Deploy GitHub Action to the GitHub Marketplace.

## Prerequisites

Before publishing, ensure you have:

1. ✅ **Public Repository**: The repository must be public
2. ✅ **action.yml**: Located in the root with proper metadata and branding
3. ✅ **README.md**: Comprehensive documentation (already created)
4. ✅ **LICENSE**: License file in the root (MIT License created)
5. ✅ **Valid action.yml branding**: Icon and color specified (already configured)

## Step-by-Step Publishing Process

### 1. Verify Repository Settings

1. Go to your repository: `https://github.com/PipeOpsHQ/pipeops-action`
2. Navigate to **Settings** → **General**
3. Ensure **Repository visibility** is set to **Public**
4. Scroll down and verify **Features** section has **Releases** enabled

### 2. Create a Release

1. Navigate to your repository on GitHub
2. Click on **Releases** (in the right sidebar or under the **Code** tab)
3. Click **Create a new release** or **Draft a new release**

### 3. Configure Release Details

Fill in the release information:

- **Choose a tag**: Create a new tag (e.g., `v1.0.0`)
  - Use semantic versioning: `v1.0.0`, `v1.1.0`, `v2.0.0`, etc.
  - For the first release, use `v1.0.0`

- **Release title**: `v1.0.0` or `PipeOps Deploy Action v1.0.0`

- **Description**: Add release notes, for example:
  ```markdown
  ## 🚀 Initial Release

  First release of the PipeOps Deploy GitHub Action.

  ### Features
  - Deploy applications to PipeOps platform
  - Support for multiple environments
  - Token-based authentication
  - JSON output parsing
  - Comprehensive error handling

  ### Usage
  ```yaml
  - uses: PipeOpsHQ/pipeops-action@v1
    with:
      pipeops-token: ${{ secrets.PIPEOPS_TOKEN }}
      project-id: 'your-project-id'
  ```
  ```

### 4. Publish to Marketplace

1. **Look for the Marketplace banner**: At the top of the release page, you'll see:
   > "Publish this Action to the GitHub Marketplace"

2. **Check the checkbox**: ✅ **Publish this Action to the GitHub Marketplace**

3. **Accept Marketplace Agreement** (first time only):
   - Read the GitHub Marketplace Agreement
   - Check the box to accept the terms
   - Click **Continue**

4. **Select Categories**:
   - **Primary Category**: Select `Deployment` or `Continuous Integration`
   - **Secondary Category** (optional): Select `Utilities` or another relevant category

5. **Review Action Details**:
   - GitHub will validate your `action.yml`
   - Verify the displayed information is correct:
     - Name: PipeOps Deploy
     - Description: Deploy applications to PipeOps platform using the PipeOps CLI
     - Icon: cloud
     - Color: blue

### 5. Publish the Release

1. Review all information
2. Click **Publish release** (or **Update release** if editing an existing one)

### 6. Verify Publication

After publishing:

1. Visit the GitHub Marketplace: `https://github.com/marketplace`
2. Search for "PipeOps" or "pipeops-action"
3. Your action should appear in the results
4. Click on it to view the Marketplace listing

## Post-Publication

### Version Management

After publishing, users can reference your action in two ways:

1. **Specific version** (recommended for stability):
   ```yaml
   uses: PipeOpsHQ/pipeops-action@v1.0.0
   ```

2. **Major version tag** (recommended for auto-updates):
   ```yaml
   uses: PipeOpsHQ/pipeops-action@v1
   ```

### Creating Version Tags

To maintain a major version tag (`v1`) that points to the latest release:

1. After creating a new release (e.g., `v1.1.0`), create a new tag `v1`:
   ```bash
   git tag -d v1  # Delete local tag if exists
   git push origin :refs/tags/v1  # Delete remote tag if exists
   git tag v1  # Create new v1 tag pointing to latest commit
   git push origin v1  # Push the tag
   ```

2. Or use GitHub's web interface:
   - Go to **Releases** → **Tags**
   - Find the latest release tag
   - Click **Edit** and update the tag name

### Updating the Action

When making updates:

1. Make your changes to `action.yml` or other files
2. Commit and push changes
3. Create a new release with incremented version:
   - Patch: `v1.0.1` (bug fixes)
   - Minor: `v1.1.0` (new features, backward compatible)
   - Major: `v2.0.0` (breaking changes)
4. Update the major version tag (`v1`) to point to the latest release
5. Publish the release to Marketplace

## Marketplace Listing Requirements

Your action must meet these requirements:

- ✅ **Public repository**
- ✅ **action.yml** in the root directory
- ✅ **name** field in action.yml
- ✅ **description** field in action.yml
- ✅ **branding** section with `icon` and `color`
- ✅ **README.md** with usage instructions
- ✅ **LICENSE** file

## Branding Icons

Available icons (from Feather Icons):
- `cloud` (current)
- `upload-cloud`
- `package`
- `rocket`
- `zap`
- `code`
- `server`
- `layers`
- `box`

Available colors:
- `white`
- `yellow`
- `blue` (current)
- `green`
- `orange`
- `red`
- `purple`
- `gray-dark`

## Troubleshooting

### "Publish to Marketplace" checkbox not visible

- Ensure the repository is **public**
- Verify `action.yml` exists in the root directory
- Check that `action.yml` has proper `name` and `description` fields
- Ensure `branding` section is present

### Validation errors

- Review error messages in the release page
- Common issues:
  - Missing required fields in `action.yml`
  - Invalid icon name (must be from Feather Icons)
  - Invalid color value
  - Missing description

### Action not appearing in Marketplace

- Wait a few minutes for indexing
- Search using different terms
- Check that the release was successfully published
- Verify the repository is public

## Best Practices

1. **Semantic Versioning**: Use `v1.0.0`, `v1.1.0`, `v2.0.0` format
2. **Release Notes**: Always include detailed release notes
3. **Major Version Tags**: Maintain `v1`, `v2` tags pointing to latest releases
4. **Testing**: Test the action thoroughly before publishing
5. **Documentation**: Keep README.md updated with latest features
6. **Changelog**: Consider maintaining a CHANGELOG.md file

## Resources

- [GitHub Marketplace Documentation](https://docs.github.com/en/actions/creating-actions/publishing-actions-in-github-marketplace)
- [Creating a GitHub Action](https://docs.github.com/en/actions/creating-actions)
- [Semantic Versioning](https://semver.org/)
- [Feather Icons](https://feathericons.com/) (for branding icons)

## Next Steps

After publishing:

1. Share the action on social media
2. Add it to your documentation
3. Create example workflows
4. Monitor usage and feedback
5. Respond to issues and feature requests
