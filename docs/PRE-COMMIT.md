# Pre-commit Hook Standards

All DarojaAI repositories use pre-commit hooks to catch issues locally before pushing.

## Installation

### For Python Repos

```bash
pip install -r requirements-dev.txt
pre-commit install
```

### For Node Repos

```bash
npm install
npx husky install  # if applicable
```

### For Terraform Repos

```bash
pip install pre-commit
pre-commit install
```

## Running Hooks

```bash
# Run on staged files (before commit)
pre-commit run

# Run on all files
pre-commit run --all-files

# Run specific hook
pre-commit run ruff --all-files

# Update hook versions (pull latest)
pre-commit autoupdate
```

## Standard Configurations

### Python repos: python.pre-commit-config.yaml

```yaml
repos:
  # Filesystem hygiene
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.5.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check-yaml
        args: [--unsafe]
      - id: check-json
      - id: check-toml
      - id: check-merge-conflict
      - id: check-added-large-files
        args: ['--maxkb=5000']
      - id: debug-statements
      - id: mixed-line-ending
        args: [--fix=lf]

  # Ruff - Python linting & formatting
  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.1.8
    hooks:
      - id: ruff
        args: [--fix, --exit-non-zero-on-fix]
      - id: ruff-format

  # MyPy - Type checking (strict)
  - repo: https://github.com/pre-commit/mirrors-mypy
    rev: v1.7.1
    hooks:
      - id: mypy
        additional_dependencies:
          - types-requests
          - types-PyYAML
        args: [--ignore-missing-imports, --disallow-untyped-defs]
        exclude: ^tests/|^venv/|^.venv/

  # Secrets detection
  - repo: https://github.com/gitleaks/gitleaks
    rev: v8.18.0
    hooks:
      - id: gitleaks

  # Checkov - IaC scanning (if Terraform present)
  - repo: https://github.com/bridgecrewio/checkov-pre-commit
    rev: 3.1.50
    hooks:
      - id: checkov
        args: [--directory, terraform/, --skip-path, terraform/.terraform]
        name: Checkov

  # YAML validation
  - repo: https://github.com/pre-commit/mirrors-yamllint
    rev: v1.33.0
    hooks:
      - id: yamllint
        args: [--strict, --allow-unicode]
        files: ^\.github/workflows/

  # Shell script validation
  - repo: https://github.com/shellcheck-py/shellcheck-py
    rev: v0.9.0.6
    hooks:
      - id: shellcheck
        args: [-x, -s, bash]

  # Docker linting
  - repo: https://github.com/hadolint/hadolint
    rev: v2.12.0
    hooks:
      - id: hadolint
        name: Hadolint
        files: ^Dockerfile$
```

### Terraform repos: terraform.pre-commit-config.yaml

```yaml
repos:
  # Filesystem hygiene
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.5.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check-yaml
      - id: check-json
      - id: check-merge-conflict
      - id: check-added-large-files
        args: ['--maxkb=5000']

  # Terraform formatting
  - repo: https://github.com/antonbabenko/pre-commit-terraform
    rev: v1.84.0
    hooks:
      - id: terraform_fmt
      - id: terraform_validate
      - id: terraform_docs
        args: [--hook-config=--path-to-file=README.md]

  # Checkov - Security scanning
  - repo: https://github.com/bridgecrewio/checkov-pre-commit
    rev: 3.1.50
    hooks:
      - id: checkov
        args: [--directory, terraform/, --skip-path, terraform/.terraform]

  # Secrets detection
  - repo: https://github.com/gitleaks/gitleaks
    rev: v8.18.0
    hooks:
      - id: gitleaks

  # YAML validation
  - repo: https://github.com/pre-commit/mirrors-yamllint
    rev: v1.33.0
    hooks:
      - id: yamllint
        args: [--strict]
        files: ^\.github/workflows/

  # Shell scripts
  - repo: https://github.com/shellcheck-py/shellcheck-py
    rev: v0.9.0.6
    hooks:
      - id: shellcheck
        args: [-x, -s, bash]
```

### Frontend repos: frontend.pre-commit-config.yaml

```yaml
repos:
  # Filesystem hygiene
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.5.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check-json
      - id: check-yaml
      - id: check-merge-conflict
      - id: check-added-large-files
        args: ['--maxkb=5000']

  # Prettier - Code formatting
  - repo: https://github.com/pre-commit/mirrors-prettier
    rev: v3.1.0
    hooks:
      - id: prettier
        types_or: [javascript, jsx, typescript, tsx, css, json, yaml]

  # ESLint - Linting
  - repo: https://github.com/pre-commit/mirrors-eslint
    rev: v8.55.0
    hooks:
      - id: eslint
        types: [javascript, jsx, typescript, tsx]
        args: [--fix]

  # Secrets detection
  - repo: https://github.com/gitleaks/gitleaks
    rev: v8.18.0
    hooks:
      - id: gitleaks

  # YAML validation (for config files)
  - repo: https://github.com/pre-commit/mirrors-yamllint
    rev: v1.33.0
    hooks:
      - id: yamllint
        files: ^\.github/
```

## What Each Hook Does

| Hook | Purpose | Auto-fix? |
|------|---------|-----------|
| **trailing-whitespace** | Remove trailing spaces | ✓ Yes |
| **end-of-file-fixer** | Ensure single newline at EOF | ✓ Yes |
| **check-yaml** | Validate YAML syntax | ✗ No |
| **check-json** | Validate JSON syntax | ✗ No |
| **check-merge-conflict** | Detect merge conflict markers | ✗ No |
| **Ruff (format)** | Auto-format Python code | ✓ Yes |
| **Ruff (lint)** | Check Python style rules | Partial |
| **MyPy** | Static type checking | ✗ No |
| **Gitleaks** | Detect secrets/credentials | ✗ No |
| **Checkov** | Security scanning (Terraform) | ✗ No |
| **terraform fmt** | Auto-format Terraform | ✓ Yes |
| **Prettier** | Auto-format JS/TS | ✓ Yes |
| **ESLint** | JavaScript/TypeScript linting | Partial |

## Troubleshooting

### "pre-commit: command not found"

```bash
# Install pre-commit globally
pip install pre-commit

# Then in repo
pre-commit install
```

### "Hook failed" but can't see why

```bash
# Run with verbose output
pre-commit run --all-files -v

# Run specific hook with debug
pre-commit run ruff --all-files --verbose
```

### Need to skip a hook temporarily

```bash
# Skip all hooks
git commit --no-verify

# Skip specific hook (config-based)
# Edit .pre-commit-config.yaml and set: stages: [manual]
```

### Hook is too strict

1. Check `.pre-commit-config.yaml` `args:` field
2. Adjust thresholds:
   ```yaml
   - id: mypy
     args: [--ignore-missing-imports]  # Add/remove options
   ```
3. Exclude problematic files:
   ```yaml
   - id: ruff
     exclude: ^tests/old_code/
   ```

## Organization-wide Updates

Hooks are versioned in `.github/.pre-commit-config.yaml`. To update all repos:

```bash
# In .github repo
pre-commit autoupdate

# Commit
git commit -am "chore: update pre-commit hooks"

# Copy to all repos
for repo in dev-nexus gcp-postgres-terraform vpc-infra; do
  cp .pre-commit-config.yaml ../DarojaAI/$repo/
  (cd ../DarojaAI/$repo && git add .pre-commit-config.yaml && git commit -m "chore: update pre-commit")
done
```

---

**Last updated:** 2026-04-28
