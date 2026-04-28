# Versioning & Release Process

All DarojaAI projects follow **Semantic Versioning 2.0** for tagged releases.

## Semantic Versioning (SemVer)

Format: `MAJOR.MINOR.PATCH`

**Example:** `v2.1.3`

### When to Bump Versions

| Type | Change | Bump |
|------|--------|------|
| **MAJOR** | Breaking API/config changes, data migrations | v1.0.0 → v2.0.0 |
| **MINOR** | New features, backward-compatible additions | v2.0.0 → v2.1.0 |
| **PATCH** | Bug fixes, security patches | v2.1.0 → v2.1.1 |

### Examples

```
v1.0.0  Initial release
v1.1.0  Added new API endpoint (backward-compatible)
v1.1.1  Fixed auth bug
v2.0.0  Removed deprecated API (breaking change)
v2.0.1  Fixed edge case in new API
v2.1.0  Added new CLI flag (backward-compatible)
```

## Version Location

Store version in your source file:

### Python (pyproject.toml)
```ini
[project]
name = "my-service"
version = "1.2.3"
```

### Node (package.json)
```json
{
  "name": "@darojaai/my-service",
  "version": "1.2.3"
}
```

### Terraform (versions.tf or variables.tf)
```hcl
locals {
  version = "1.2.3"
}
```

### Go (main.go or VERSION file)
```go
const Version = "1.2.3"
```

## Release Process

### Step 1: Plan Changes

Gather commits since last release:
```bash
git log v1.2.0..HEAD --oneline
```

Determine version bump (MAJOR / MINOR / PATCH).

### Step 2: Bump Version in Source

**Python:**
```bash
# Update pyproject.toml
version = "1.3.0"
```

**Node:**
```bash
npm version minor  # Auto-bumps package.json & creates tag
# OR manually edit package.json
```

**Terraform:**
```hcl
locals {
  version = "1.3.0"  # In main.tf or versions.tf
}
```

### Step 3: Create PR

```bash
git checkout -b release/v1.3.0
git add pyproject.toml package.json terraform/versions.tf
git commit -m "chore: bump version to 1.3.0"
git push origin release/v1.3.0
```

**PR title:** `chore: release v1.3.0`

**PR body:**
```markdown
## Release v1.3.0

### Features
- Added new CLI flag `--debug` for troubleshooting

### Fixes
- Fixed auth timeout on slow networks

### Breaking Changes
None

### Deployment Notes
- Requires database migration (auto-run on deploy)
- Update dependent services to use new config key `service_url` (old `endpoint` still works)
```

### Step 4: Merge to Main

- ✅ Pre-commit passes
- ✅ CI passes
- ✅ 1 review approval
- Merge with "Create a merge commit" (not squash)

### Step 5: Auto-Release (GitHub Actions)

When PR merges, GitHub Actions automatically:

1. Detects version bump in source file
2. Creates Git tag (e.g., `v1.3.0`)
3. Generates release notes from commit messages
4. Creates GitHub Release with notes
5. Publishes to package registry (if applicable)

**No manual steps needed — it's automatic!**

### Verify Release

```bash
# Check GitHub Releases page
https://github.com/DarojaAI/[repo]/releases

# Or via CLI
gh release list --repo DarojaAI/[repo]
```

## Pre-Release & Hotfixes

### Pre-releases (alpha/beta/rc)

Format: `v1.3.0-beta.1`

```bash
# In pyproject.toml / package.json
version = "1.3.0-beta.1"

# Create PR, merge, auto-tag
# GitHub Actions marks as "pre-release"
```

### Hotfixes (patch from old version)

For critical bugs after release:

```bash
# Create from tag
git checkout v1.2.0
git checkout -b hotfix/v1.2.1

# Fix, bump version
# Create PR to main
git commit -m "fix: critical security issue"
# Merge → auto-tags v1.2.1
```

## Deployment & CDN

### Published Artifacts

| Repo Type | Artifact | Registry |
|-----------|----------|----------|
| Python package | .whl | PyPI (if public) |
| Terraform module | Git tag + changelog | GitHub Releases |
| Node package | .tgz | npm (if public) |
| Docker image | image:tag | Docker Hub / GCR |
| Frontend app | Static assets | CDN (if applicable) |

### Version in Deployment

After release, reference version in deployments:

```bash
# Terraform: pin module version
source = "git::https://github.com/DarojaAI/vpc-infra.git//terraform?ref=v1.0.5"

# Python: specify version
pip install my-service==1.2.3

# Node: use from package.json
npm install @darojaai/my-service@1.2.3
```

## Changelog (Optional)

If you maintain a CHANGELOG.md:

```markdown
## [1.3.0] - 2026-04-28

### Added
- New CLI flag `--debug`

### Fixed
- Auth timeout on slow networks (#42)

### Changed
- Deprecated `endpoint` config (use `service_url`)

### Security
- Updated vulnerable dependency (CVE-2026-1234)

[1.3.0]: https://github.com/DarojaAI/repo/compare/v1.2.0...v1.3.0
```

Note: GitHub Actions auto-generates release notes, so manual CHANGELOG is optional.

## Troubleshooting

### Release didn't auto-tag

**Cause:** Version file not detected in push path.

**Fix:** Check `.github/workflows/release.yml` `paths:` field:
```yaml
paths: [pyproject.toml]  # Add your version file
```

### Wrong version bumped

**Rollback:**
```bash
# Delete tag locally
git tag -d v1.3.0

# Delete on GitHub
gh release delete v1.3.0 --confirm

# Fix version in source, create new PR
```

### Need to re-release (version already exists)

**Approach 1:** Bump patch version again
```
v1.3.0 → v1.3.1 (fix the fix)
```

**Approach 2:** Delete tag and re-push
```bash
git tag -d v1.3.0
gh release delete v1.3.0 --confirm
# Push same commit again (CI re-runs, new release created)
```

## Best Practices

1. **Bump version first** — Commit version change, then merge, then release auto-tags
2. **Use descriptive PRs** — Release notes auto-generated from commits
3. **Tag discipline** — One version per Git tag, never re-use
4. **Communicate breaks** — MAJOR bumps require migration guide
5. **Test releases** — Verify published artifact works before announcing

---

**Last updated:** 2026-04-28
**Reference:** https://semver.org/
