# CI/CD Adoption Status by Repo

Last updated: 2026-04-28

## Repository Status Matrix

| Repo | Workflows | Pre-commit | PR Template | Branch Protection | Version Check | Status |
|------|-----------|------------|-------------|-------------------|---------------|--------|
| **dev-nexus** | ✅ 11 | ✅ Python | ❌ | ❌ | ❌ | 🟡 IN-PROGRESS |
| **dev-nexus-frontend** | ✅ 3 | ❌ | ❌ | ❌ | ❌ | 🔴 TODO |
| **gcp-postgres-terraform** | ✅ 4 | ❌ | ❌ | ❌ | ❌ | 🔴 TODO |
| **vpc-infra** | ❌ | ❌ | ❌ | ❌ | ❌ | 🔴 TODO |
| **bond-nexus** | ❌ | ❌ | ❌ | ❌ | ❌ | 🔴 TODO |
| **research-orchestrator** | ✅ 1 | ❌ | ❌ | ❌ | ❌ | 🔴 TODO |
| **linux-desktop-seed** | ❌ | ❌ | ❌ | ❌ | ❌ | 🔴 TODO |
| **linux-desktop-seed-public** | ❌ | ❌ | ❌ | ❌ | ❌ | 🔴 TODO |
| **rag_research_tool** | ❌ | ❌ | ❌ | ❌ | ❌ | 🔴 TODO |
| **gcp-vpc-egress-terraform** | ❌ | ❌ | ❌ | ❌ | ❌ | 🔴 TODO |
| ... (15 more repos) | | | | | | |

## Status Legend

- 🟢 **COMPLETE** — All standards met
- 🟡 **IN-PROGRESS** — Some standards met, adoption underway
- 🔴 **TODO** — Not yet started
- ⚫ **BLOCKED** — Dependencies prevent progress

## Priority Tiers

### Tier 1: Production Services (Week 2-3)
**High impact, high user exposure**

- [ ] dev-nexus (service + infrastructure)
- [ ] gcp-postgres-terraform (production database)
- [ ] vpc-infra (production networking)

### Tier 2: Active Development (Week 3-4)
**Medium impact, actively developed**

- [ ] dev-nexus-frontend (production UI)
- [ ] research-orchestrator (data pipeline)
- [ ] bond-nexus (service)

### Tier 3: Lower Priority (Week 4-5)
**Lower user impact or less active**

- [ ] linux-desktop-seed*
- [ ] rag_research_tool
- [ ] gcp-vpc-egress-terraform
- [ ] gcp-dbt-terraform
- ... (9 more repos)

## Per-Repo Details

### dev-nexus

```
Current: ✅ Workflows, ✅ Pre-commit (Python), ❌ PR template, ❌ Branch protection, ❌ Version check
Gaps: 3 items
Owner: @TBD
Timeline: Week 2
```

**Tasks:**
- [ ] Add .github/pull_request_template.md
- [ ] Enable branch protection (1 review + status checks)
- [ ] Add version-check workflow

**PR Board:** [Link]

---

### dev-nexus-frontend

```
Current: ✅ Workflows, ❌ Pre-commit, ❌ PR template, ❌ Branch protection, ❌ Version check
Gaps: 4 items
Owner: @TBD
Timeline: Week 3
```

**Tasks:**
- [ ] Copy frontend.pre-commit-config.yaml
- [ ] Add .github/pull_request_template.md
- [ ] Enable branch protection
- [ ] Add version-check workflow

---

### gcp-postgres-terraform

```
Current: ✅ Workflows (terraform), ❌ Pre-commit, ❌ PR template, ❌ Branch protection, ❌ Version check
Gaps: 4 items
Owner: @TBD
Timeline: Week 2
```

**Tasks:**
- [ ] Copy terraform.pre-commit-config.yaml
- [ ] Add .github/pull_request_template.md
- [ ] Enable branch protection
- [ ] Add version-check workflow

---

### vpc-infra

```
Current: ❌ All
Gaps: 5 items
Owner: @TBD
Timeline: Week 2-3
```

**Tasks:**
- [ ] Add .github/workflows (terraform-plan.yml, terraform-apply.yml, release.yml)
- [ ] Copy terraform.pre-commit-config.yaml
- [ ] Add .github/pull_request_template.md
- [ ] Enable branch protection
- [ ] Add version-check workflow

---

_Each repo gets its own detailed status doc: docs/CI-CD-ADOPTION-STATUS.md_

## Completion Timeline

```
Week 1 (Done)
  ✅ Create DarojaAI/.github
  ✅ Document standards
  ✅ Create templates

Week 2
  🔄 Tier 1 repos (dev-nexus, gcp-postgres-terraform, vpc-infra)
  🔄 Create per-repo PRs
  
Week 3
  🔄 Tier 2 repos (frontend, orchestrator, bond-nexus)
  
Week 4
  🔄 Tier 3 repos (others)
  ✅ Enforce branch protection org-wide
  
Week 5+
  🔄 Monitor & maintain
  📅 Quarterly reviews
```

## Adoption Metrics

| Metric | Current | Target | ETA |
|--------|---------|--------|-----|
| Repos with workflows | 8/23 | 23/23 | Week 4 |
| Repos with pre-commit | 1/23 | 23/23 | Week 4 |
| Repos with PR template | 0/23 | 23/23 | Week 2 |
| Repos with branch protection | 0/23 | 23/23 | Week 4 |
| Repos with auto-versioning | 0/23 | 23/23 | Week 3 |

---

**Maintained by:** Infrastructure Team
**Last sync:** 2026-04-28 20:03 UTC
