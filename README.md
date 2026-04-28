# DarojaAI Organization Standards

This repository contains organization-wide standards, templates, and governance for all DarojaAI repositories.

## 📖 Quick Links

**Standards:**
- [CONTRIBUTING.md](./CONTRIBUTING.md) — How to contribute to any DarojaAI repo
- [GOVERNANCE.md](./GOVERNANCE.md) — Organization structure and decision-making
- [docs/CI-CD-STANDARDS.md](./docs/CI-CD-STANDARDS.md) — Mandatory CI/CD practices
- [docs/VERSIONING.md](./docs/VERSIONING.md) — Semantic versioning and release process
- [docs/BRANCH-PROTECTION.md](./docs/BRANCH-PROTECTION.md) — PR and review requirements
- [docs/PRE-COMMIT.md](./docs/PRE-COMMIT.md) — Pre-commit hook standards

**Templates:**
- [pull_request_template.md](./pull_request_template.md) — PR template (auto-applied to all repos)
- [.pre-commit-config.yaml](./.pre-commit-config.yaml) — Shared hook versions
- [workflow-templates/](./workflow-templates/) — Reusable GitHub Actions

**Repo Status:**
- [docs/ADOPTION-STATUS.md](./docs/ADOPTION-STATUS.md) — Per-repo adoption tracking

## 🚀 For New Repos

1. Create repo in GitHub
2. Choose template type: **Python** | **Terraform** | **Frontend**
3. Copy files from this repo (see [docs/NEW-REPO-TEMPLATE.md](./docs/NEW-REPO-TEMPLATE.md))
4. Fill in config files (pyproject.toml / package.json / terraform.tf)
5. Push and enable branch protection
6. Mark adoption status as COMPLETE

Estimated time: **15 minutes**

## 🔄 For Existing Repos

Follow [docs/ADOPTION-PLAN.md](./docs/ADOPTION-PLAN.md) phased rollout (4 weeks).

## 💬 Questions?

File an issue in this repo or ask in #dev-nexus-standards (Discord).

---

**Last updated:** 2026-04-28
**Maintained by:** DarojaAI Infrastructure Team
