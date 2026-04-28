# Branch Protection Rules

All DarojaAI repositories enforce branch protection on `main` to prevent accidental merges of untested code.

## Standard Rules (Applied to All Repos)

### Dismiss stale pull request approvals when new commits are pushed

**Why:** Ensures reviewers verify latest changes.

```
✓ Enabled
```

### Require pull request reviews before merging

```
✓ Required approving reviews: 1
✓ Require review from Code Owners (if CODEOWNERS file exists)
✓ Dismiss stale reviews on push
✓ Require approval of the most recent reviewable push
```

### Require status checks to pass before merging

```
✓ Require branches to be up to date before merging
✓ Status checks (must pass):
  - ci (or workflow name)
  - pre-commit
  - (language-specific: pytest, eslint, terraform validate, etc.)
```

### Enforce all the above settings for administrators

```
✓ Enabled (admins cannot bypass rules)
```

## Optional Enhancements

### Require code owner review

Use only if you have a `CODEOWNERS` file:

```
✓ Require Code Owner review
✓ Require review from CODEOWNERS (if applicable)
```

**Example CODEOWNERS:**
```
* @infrastructure-team
docs/ @docs-team
src/api/ @api-team
terraform/ @infrastructure-team
```

### Require up-to-date before merging

```
✓ Enabled (branches must be rebased/synced with main before merge)
```

This prevents merge conflicts and ensures all checks pass on the latest `main`.

### Require signed commits

```
✓ Enabled (optional, for high-security repos)
```

Requires GPG-signed commits (more restrictive, slower workflow).

### Require conversation resolution before merging

```
✓ Enabled (all review comments must be resolved)
```

Ensures reviewers address feedback.

## Implementation

### Via GitHub UI

1. Go to repo → Settings → Branches
2. Select "main" branch
3. Enable rule: "Require a pull request before merging"
4. Enable: "Require 1 approval"
5. Enable: "Require status checks to pass"
6. Select status checks:
   - `ci`
   - `pre-commit`
   - Language-specific (pytest, eslint, terraform validate)
7. Enable: "Require branches to be up to date"
8. Enable: "Include administrators"

### Via GitHub CLI

```bash
# Enable branch protection on main
gh api repos/DarojaAI/[repo]/branches/main/protection \
  -X PUT \
  -f required_status_checks='{"strict": true, "contexts": ["ci", "pre-commit"]}' \
  -f required_pull_request_reviews='{"required_approving_review_count": 1, "dismiss_stale_reviews": true}' \
  -f enforce_admins=true \
  -f allow_force_pushes=false \
  -f allow_deletions=false
```

### Bulk Script (All Repos)

```bash
#!/bin/bash
REPOS=(
  "dev-nexus"
  "gcp-postgres-terraform"
  "vpc-infra"
  "bond-nexus"
  "dev-nexus-frontend"
)

for repo in "${REPOS[@]}"; do
  echo "Setting branch protection for DarojaAI/$repo"
  gh api repos/DarojaAI/$repo/branches/main/protection \
    -X PUT \
    -f required_status_checks='{"strict": true, "contexts": ["ci", "pre-commit"]}' \
    -f required_pull_request_reviews='{"required_approving_review_count": 1, "dismiss_stale_reviews": true}' \
    -f enforce_admins=true \
    -f allow_force_pushes=false \
    -f allow_deletions=false
done
```

## Bypass Scenarios (Emergency Only)

### When to bypass

- **Critical security hotfix:** Can't wait for review
- **Production outage:** Need immediate fix
- **Urgent deployment:** Time-sensitive change

### How to bypass

1. **Push to protected branch directly:**
   ```bash
   # Admins only (after creating exception)
   git push --force-with-lease origin main
   ```

2. **Create release with override:**
   ```bash
   gh release create v1.0.0 --title "Hotfix" --notes "Emergency fix"
   ```

3. **Request temporary disable:**
   Contact infrastructure team in Discord
   - Provide reason
   - Will re-enable immediately after

## Monitoring

### Check status

```bash
# View protection rules
gh api repos/DarojaAI/[repo]/branches/main/protection

# View recent bypasses (audit log)
gh api repos/DarojaAI/[repo]/audit-log --jq '.[].data'
```

### Common Issues

**"Branch protection rule violation"**
→ PR needs 1 approval + all checks passing

**"Latest commit not approved"**
→ Stale review dismissed, needs new approval

**"Required status check missing"**
→ Workflow didn't run or failed (check Actions tab)

---

**Last updated:** 2026-04-28
