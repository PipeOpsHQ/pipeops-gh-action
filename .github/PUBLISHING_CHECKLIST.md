# Publishing Checklist

Quick checklist before publishing to GitHub Marketplace:

## Pre-Publication Checklist

- [ ] Repository is **public**
- [ ] `action.yml` exists in root directory
- [ ] `action.yml` has `name` field ✅
- [ ] `action.yml` has `description` field ✅
- [ ] `action.yml` has `branding` section with `icon` and `color` ✅
- [ ] `README.md` exists with usage examples ✅
- [ ] `LICENSE` file exists (MIT License) ✅
- [ ] Action has been tested locally
- [ ] All inputs/outputs are documented in README

## Publishing Steps

1. [ ] Go to repository → **Releases** → **Create a new release**
2. [ ] Create tag: `v1.0.0`
3. [ ] Add release title and description
4. [ ] ✅ Check **"Publish this Action to the GitHub Marketplace"**
5. [ ] Accept Marketplace Agreement (first time)
6. [ ] Select category: **Deployment** or **Continuous Integration**
7. [ ] Click **Publish release**
8. [ ] Verify action appears in Marketplace

## Post-Publication

- [ ] Create major version tag `v1` pointing to latest release
- [ ] Update documentation with Marketplace link
- [ ] Share on social media/documentation

## Quick Commands

```bash
# Create and push version tag
git tag v1.0.0
git push origin v1.0.0

# Create/update major version tag
git tag -d v1
git tag v1
git push origin v1 --force
```
