# DarojaAI Organization Standards

This repository contains organization-wide standards, templates, and governance for all DarojaAI repositories.

## What this repo contains

GitHub gives `<org>/.github` a special role: it's where org-wide GitHub mechanics live (community health files, profile README, templates other repos copy). DarojaAI uses `.github` for *two* things, by design — both visible from this single repo:

**1. GitHub mechanics** (GitHub auto-applies or other repos copy these):
- `CONTRIBUTING.md` — default contributing guide for every repo in the org (auto-applied by GitHub)
- `pull_request_template.md` — auto-populates new PRs across the org
- `.pre-commit-config.yaml` — shared hook versions that other repos copy
- `workflow-templates/` — reusable GitHub Actions templates that other repos copy when bootstrapping
- `profile/README.md` — renders on the [DarojaAI GitHub org page](https://github.com/DarojaAI)

**2. Org-wide policy docs** (read by humans and agents when they need the rule):
- `GOVERNANCE.md` — repository categories, decision-making, security, compliance, roles
- `docs/CI-CD-STANDARDS.md` — mandatory CI/CD practices
- `docs/PRE-COMMIT.md` — pre-commit hook standards
- `docs/VERSIONING.md` — semantic versioning and release process
- `docs/BRANCH-PROTECTION.md` — PR and review requirements
- `docs/ADOPTION-STATUS.md` — per-repo adoption tracking
- `docs/AGENTS-TEMPLATE.md` — the org standard for `AGENTS.md` in every active repo

**Why one repo for both?** At the org's current size (42 repos, one operator), splitting mechanics from policy means a new repo to maintain and a long tail of cross-reference fixes. Not worth it until there's a Platform team with separate access needs. The dual role is documented at the top of this file so contributors learn it in 30 seconds.

## 📖 Quick Links

**Standards:**
- [CONTRIBUTING.md](./CONTRIBUTING.md) — How to contribute to any DarojaAI repo
- [GOVERNANCE.md](./GOVERNANCE.md) — Organization structure and decision-making
- [docs/CI-CD-STANDARDS.md](./docs/CI-CD-STANDARDS.md) — Mandatory CI/CD practices
- [docs/VERSIONING.md](./docs/VERSIONING.md) — Semantic versioning and release process
- [docs/BRANCH-PROTECTION.md](./docs/BRANCH-PROTECTION.md) — PR and review requirements
- [docs/PRE-COMMIT.md](./docs/PRE-COMMIT.md) — Pre-commit hook standards
- [docs/AGENTS-TEMPLATE.md](./docs/AGENTS-TEMPLATE.md) — Org standard for `AGENTS.md` in every active repo

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

**Last updated:** 2026-06-17
**Maintained by:** `@no_decaf_milan` (de-facto org owner; designate per-area maintainers as the Platform team forms per RFC #2)
