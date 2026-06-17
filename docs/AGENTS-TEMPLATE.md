# AGENTS.md Template

> **Org standard for DarojaAI repos.** Copy this file into your repo as `AGENTS.md` and customize the bracketed placeholders. Required sections are marked **(required)**; everything else is optional or category-specific.

This template is the minimum required structure per `.github` RFC #1 (approved 2026-06-14). The full RFC is referenced at the bottom.

---

## How to use this template

1. Copy this file to `<your-repo>/AGENTS.md`.
2. Replace every `[bracketed placeholder]` with repo-specific content.
3. Keep the six required sections in order.
4. Add optional category-specific sections at the end.
5. Don't add content that belongs in `README.md` (humans) — see "Prohibited content" below.

---

# [Repo Name] — Agents

[One-paragraph role description. What is this repo, and what is an agent's job when working in it? Cite the relevant `GOVERNANCE.md` category. **Required**]

## First Run

[If `BOOTSTRAP.md` exists in the repo, follow it, then delete it. Otherwise, this section can be a one-liner. **Required**]

## Session Startup

[What to read first when an agent session starts. The runtime already provides AGENTS.md, SOUL.md, USER.md, and recent memory — do not duplicate that. Reference it; do not re-list it. **Required**]

## Project Quick Context

[A 5-10 line bullet list pointing to the load-bearing docs. **Required**]

- **Architecture:** `docs/ARCHITECTURE.md` (or equivalent)
- **Components:** `docs/COMPONENTS.md` (or equivalent)
- **Dev setup:** `README.md` or `docs/SETUP.md`
- **Env vars:** [link, do not enumerate]
- **Decisions / lessons:** `docs/decisions/` or `MEMORY.md` (if present)
- **Boundary contract (if applicable):** the relevant P0 doc, e.g. `dev-nexus/docs/architecture/architectural-boundaries.md`

## House Rules

[5-10 bullets of repo-specific rules an agent MUST follow. **Required**]

- [Example: "Don't bypass Terraform CI for state mutations."]
- [Example: "Don't commit secrets; pre-commit includes Gitleaks."]
- [Example: "Don't push directly to main; PR required."]
- [Example: "Use the org-wide [CI-CD-STANDARDS §1 no-hardcoded-values rule](./CI-CD-STANDARDS.md)."]

## Related

[5-10 links to the upstream/downstream repos and the P0 boundary contract (if applicable). **Required**]

- [Org governance: `DarojaAI/.github/GOVERNANCE.md`](https://github.com/DarojaAI/.github/blob/main/GOVERNANCE.md)
- [Boundary contract (if applicable): <link>]
- [Upstream: <repo>](<link>)
- [Downstream: <repo>](<link>)

---

## Optional: category-specific sections

Pick the section that matches your repo's category per `GOVERNANCE.md`. Delete the others.

### Work Completion Gate *(Infrastructure / Terraform repos)*

Pre-commit, fmt, init, validate, checkov. **PR via `.github` not direct apply.** [Add your repo's specific gates here.]

### Behavioral Anchors *(Core Services / Python repos)*

What high agency means in this repo. Tests/coverage gate. [Add your repo's specific anchors here.]

### Build & Deploy *(Frontend / TypeScript repos)*

Vite/CRA/Next build, lint, type-check, deploy target. [Add your repo's specific build commands here.]

### Audience *(Public / Examples repos)*

Who the example is for. If you copy this repo, also read [X]. [Add your repo's specific audience notes here.]

### Skill Organization *(Shared Libraries & Templates repos)*

If this repo contains skills (e.g. `skill_workshop/<name>/SKILL.md`), name the skill author, the consumers, and any related skill domains. See [RFC 0001 (darojaai_architect)](https://github.com/DarojaAI/darojaai_architect) for the three-layer skill model.

---

## Prohibited content

These belong in `README.md` (humans), not `AGENTS.md` (agents):

- Marketing copy / "Why this project is great"
- Long changelogs (use `CHANGELOG.md`)
- Setup tutorials for non-agents
- Pricing, business model, fundraising
- Screenshots of the UI

If you find yourself writing these, you're in the wrong file.

---

## Template metadata

- **Source:** `DarojaAI/.github/docs/AGENTS-TEMPLATE.md`
- **Source RFC:** [`.github#1`](https://github.com/DarojaAI/.github/issues/1) (Standardized AGENTS.md template, approved 2026-06-14)
- **Owner:** `@no_decaf_milan`
- **Last updated:** 2026-06-17
