# CI/CD Standards

All DarojaAI repositories must follow these CI/CD practices.

## Mandatory for All Repos

### 1. GitHub Actions Workflows

**Required:**
- `ci.yml` — Runs on every PR to main
- `release.yml` — Runs on merge to main (auto-tags)

**Content depends on repo type:**

| Type | CI Checks | Release Action |
|------|-----------|---|
| Python | pytest + coverage | Create GitHub release, publish to PyPI (if applicable) |
| Terraform | terraform validate + checkov | Create GitHub release with changelog |
| TypeScript | eslint + tsc + tests | Create GitHub release, publish to npm (if applicable) |
| Other | Linting + tests | Create GitHub release |

**Status checks must pass before merge:**
- All workflow checks (✓ green)
- Pre-commit hooks (no issues)
- Tests (≥80% coverage if Python)

### 2. Pre-commit Hooks

**All repos must have:**
```yaml
.pre-commit-config.yaml
```

**Minimum hooks:**
- Filesystem hygiene (trailing-whitespace, end-of-file-fixer, check-merge-conflict)
- YAML validation (check-yaml, yamllint)
- Secrets detection (Gitleaks)
- Language-specific linting (Ruff for Python, ESLint for TS, terraform fmt for IaC)

**Organization-provided config:**
- Python: [python.pre-commit-config.yaml](../docs/PRE-COMMIT.md)
- Terraform: [terraform.pre-commit-config.yaml](../docs/PRE-COMMIT.md)
- Frontend: [frontend.pre-commit-config.yaml](../docs/PRE-COMMIT.md)

### 3. Branch Protection

**All main branches require:**

```
✓ Pull request review (1 approval minimum)
✓ Status checks required:
  - All workflow checks must pass
  - Pre-commit validation
  - Secrets scanning
✓ Branches must be up to date
✓ Dismiss stale reviews on new commits
✓ Enforce for administrators too
```

### 4. Versioning

**See [VERSIONING.md](./VERSIONING.md) for details.**

- Use semantic versioning (MAJOR.MINOR.PATCH)
- Version in source file (pyproject.toml / package.json / terraform.tf)
- GitHub Actions auto-tags on version bump
- Releases auto-generated from commit messages

## Python Repos

### Pre-commit Hooks

```yaml
- Ruff (linting + formatting)
- MyPy (strict type checking)
- Checkov (if Terraform present)
- Gitleaks (secrets)
```

### CI Workflow (ci.yml)

```yaml
on: [push, pull_request]

jobs:
  tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v4
        with:
          python-version: "3.11"
          cache: pip
      
      - name: Install dependencies
        run: |
          pip install -r requirements-dev.txt
          pre-commit install
      
      - name: Run pre-commit
        run: pre-commit run --all-files
      
      - name: Run tests
        run: pytest --cov=src tests/
      
      - name: Check coverage
        run: pytest --cov=src --cov-fail-under=80 tests/
```

### Release Workflow (release.yml)

```yaml
on:
  push:
    branches: [main]
    paths: [pyproject.toml]  # Trigger on version bump

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Get version
        id: version
        run: |
          VERSION=$(grep 'version =' pyproject.toml | cut -d'"' -f2)
          echo "version=v$VERSION" >> $GITHUB_OUTPUT
      
      - name: Create release
        uses: softprops/action-gh-release@v1
        with:
          tag_name: ${{ steps.version.outputs.version }}
          generate_release_notes: true
```

## Terraform Repos

### Pre-commit Hooks

```yaml
- terraform fmt
- Checkov (security scanning)
- yamllint (for CI/CD files)
- Gitleaks (secrets)
```

### CI Workflow (terraform-plan.yml)

```yaml
on: [pull_request]

jobs:
  terraform:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: hashicorp/setup-terraform@v2
      
      - name: Terraform Format Check
        run: terraform fmt -check -recursive terraform/
      
      - name: Terraform Init
        run: terraform -chdir=terraform init -backend=false
      
      - name: Terraform Validate
        run: terraform -chdir=terraform validate
      
      - name: Checkov Security Scan
        uses: bridgecrewio/checkov-action@master
        with:
          directory: terraform/
          framework: terraform
```

### Release Workflow (release.yml)

```yaml
on:
  push:
    branches: [main]
    paths: [terraform/versions.tf]

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Create release
        uses: softprops/action-gh-release@v1
        with:
          generate_release_notes: true
```

## Frontend Repos

### Pre-commit Hooks

```yaml
- Prettier (formatting)
- ESLint (linting)
- TypeScript type-check
- Gitleaks (secrets)
```

### CI Workflow (ci.yml)

```yaml
on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: "20"
          cache: npm
      
      - name: Install
        run: npm ci
      
      - name: Lint
        run: npm run lint
      
      - name: Type check
        run: npm run type-check
      
      - name: Test
        run: npm test -- --run
      
      - name: Build
        run: npm run build
```

## Status Checks

**Required checks (all repos):**
- `ci` (or workflow name) — All tests/linting pass
- `pre-commit` — Hooks run clean

**Optional enhancements:**
- `coverage` — Codecov integration
- `security` — Snyk or Trivy scan
- `dependency-check` — Dependabot or Renovate

## Secrets & Security

### No Hardcoded Values

- Database credentials → GitHub Secrets + GitHub Actions `environment`
- API keys → GitHub Secrets
- SSH keys → GitHub Secrets

### Gitleaks Scanning

All repos run Gitleaks pre-commit and in CI to detect secrets in commits.

### Dependency Scanning

- Enable Dependabot (auto-enabled on GitHub)
- Review security alerts weekly
- Merge Dependabot PRs with pre-commit passing

## Enforcement

### Monthly Audit

Infrastructure team runs:
```bash
# Check all repos
for repo in $(gh repo list DarojaAI --jq '.[] | .name'); do
  echo "=== $repo ==="
  # Verify workflows, branch protection, pre-commit
done
```

### Noncompliance

1. **First notice:** Issue filed, owner notified
2. **30 days:** Review and remediate
3. **60 days:** Escalate if unaddressed

---

**Last updated:** 2026-04-28
**Review schedule:** Quarterly
