# Contributing to DarojaAI Projects

Thank you for contributing! All DarojaAI repositories follow these organization-wide standards.

## Before You Start

1. Read [GOVERNANCE.md](./GOVERNANCE.md) to understand our structure
2. Check the specific repo's `CONTRIBUTING.md` (often just inherits this file)
3. Review [docs/CI-CD-STANDARDS.md](./docs/CI-CD-STANDARDS.md) for required checks
4. **For agents (LLM sessions):** if your repo has an `AGENTS.md`, read it first. The org template is at [docs/AGENTS-TEMPLATE.md](./docs/AGENTS-TEMPLATE.md) — every active repo should have an `AGENTS.md` derived from it (per `.github` RFC #1).

## Development Workflow

### 1. Fork & Clone

```bash
git clone https://github.com/DarojaAI/[repo-name].git
cd [repo-name]
```

### 2. Install Pre-commit

All repos use pre-commit hooks to catch issues locally before pushing:

```bash
# Python repos
pip install -r requirements-dev.txt
pre-commit install

# Node repos
npm install
npx husky install  # if applicable

# Terraform repos
pre-commit install
```

### 3. Make Changes

```bash
git checkout -b feature/your-feature-name
# Make changes
git add .
git commit -m "feat: add new feature"
```

**Commit message format:** 
- `feat:` New feature
- `fix:` Bug fix
- `docs:` Documentation
- `refactor:` Code restructuring
- `test:` Test additions
- `chore:` Maintenance

### 4. Push & Create PR

```bash
git push origin feature/your-feature-name
# Open PR on GitHub
```

**PR requirements:**
- ✅ Pre-commit checks pass
- ✅ Tests pass (CI)
- ✅ 1 code review approval
- ✅ Changelog/version bump (if applicable)
- ✅ No merge conflicts

### 5. Merge

Merges to `main` require:
1. ✅ Passing CI/CD checks
2. ✅ Minimum 1 approval
3. ✅ Branch protection enforced

## Code Standards

### Python

- **Linter:** Ruff
- **Formatter:** Ruff
- **Type checker:** MyPy (strict mode)
- **Tests:** pytest with ≥80% coverage

```bash
pre-commit run --all-files
pytest tests/ --cov
```

### TypeScript/JavaScript

- **Linter:** ESLint
- **Formatter:** Prettier
- **Tests:** Vitest / Jest

```bash
npm run lint
npm run type-check
npm test
```

### Terraform

- **Formatter:** terraform fmt
- **Linter:** Checkov (security)
- **Validator:** terraform validate
- **Tests:** terratest (if applicable)

```bash
terraform fmt -recursive
checkov -d terraform/
```

## Security

- **No secrets in commits:** Pre-commit includes Gitleaks scanning
- **No hardcoded values:** Use environment variables or GitHub secrets
- **Dependency updates:** Use Dependabot (auto-enabled)
- **Security scanning:** Trivy for container images, Checkov for IaC

## Versioning & Releases

1. **Bump version** in `pyproject.toml` / `package.json` / Terraform module version
2. **Create PR** with version bump
3. **Merge to main**
4. **GitHub Actions auto-creates release** with generated notes

See [docs/VERSIONING.md](./docs/VERSIONING.md) for details.

## Branch Protection Rules

All `main` branches enforce:

```
✓ Require PR review (1 approval minimum)
✓ Require passing CI/CD checks
✓ Require branches up-to-date
✓ Dismiss stale reviews on new commits
✓ Enforce for admins too
```

## Questions?

1. Check repo-specific `CONTRIBUTING.md`
2. Review [GOVERNANCE.md](./GOVERNANCE.md)
3. Open an issue in this repo
4. Ask in Discord (#dev-nexus-standards)

---

## Agent Documentation Standard

Every active DarojaAI repo **must** have an `AGENTS.md` at the repo root, derived from the org template at [docs/AGENTS-TEMPLATE.md](./docs/AGENTS-TEMPLATE.md). The required sections are:

1. **Role** (1 paragraph) — cite the relevant `GOVERNANCE.md` category
2. **First Run** — handle `BOOTSTRAP.md` if it exists, otherwise one-liner
3. **Session Startup** — what to read first
4. **Project Quick Context** — 5-10 bullets pointing to load-bearing docs
5. **House Rules** — 5-10 repo-specific rules
6. **Related** — 5-10 links to upstream/downstream repos and any P0 boundary contract

Optional category-specific extensions are described in the template. Repos may add additional sections, but the six required sections must appear in order.

**Adoption check:** the org-wide adoption status is tracked in [docs/ADOPTION-STATUS.md](./docs/ADOPTION-STATUS.md). If your repo is listed as "missing AGENTS.md" or "non-conformant," the next `AGENTS.md` change should bring it into compliance.

**PR template:** the [pull_request_template.md](./pull_request_template.md) includes a "Documentation" checklist item. AGENTS.md changes that don't conform to the template are a CI failure once the org-wide lint is in place (future work, see `.github#1` Phase 2).

---

**Last updated:** 2026-06-17
