# DarojaAI Governance

## Organization Structure

**DarojaAI** is organized around products and infrastructure. Each repo has an owner/team responsible for its standards compliance.

### Repository Categories

1. **Infrastructure (IaC)**
   - `vpc-infra`, `gcp-postgres-terraform`, `gcp-dbt-terraform`, etc.
   - Owner: Infrastructure team
   - Standards: Terraform linting, security scanning (Checkov), version tagging

2. **Core Services**
   - `dev-nexus`, `bond-nexus`, `research-orchestrator`, etc.
   - Owner: Product teams
   - Standards: CI/CD, pre-commit, version bumping, automated releases

3. **Frontend Applications**
   - `dev-nexus-frontend`, `business-website`, etc.
   - Owner: Frontend team
   - Standards: TypeScript strict mode, ESLint, Prettier, tests

4. **Public/Examples**
   - `gcp-postgres-terraform-example`, etc.
   - Owner: Documentation/advocacy team
   - Standards: Clear README, active maintenance, responsive to issues

5. **Shared Libraries & Templates** *(approved 2026-06-14, .github#2)*
   - `infra-actions` — composite GitHub Actions used across the org
   - `devnexus-common` (proposed rename: `py-daroja-libs`) — shared Python utilities (LLM client, tracing)
   - `daroja-frontend-starter` — Vite 19 + Cloudflare template for new frontends
   - `intelligent-feed` — per-project activators consumed by `research-orchestrator` and others
   - `skill-bridge` — Claude Code skills → OpenClaw translator
   - Owner: `@no_decaf_milan` (de-facto; designate per-lib maintainers as adoption matures)
   - Standards: **semantic versioning enforced, release on tag, public changelog, documented consumer upgrade path, CI must be green on all supported consumer repos when the lib cuts a major.** Different release cadence and ownership model than products (a library has *consumers*, multiple; a breaking change is *every consumer's* pain).

## Decision-Making

### Standards Changes

**Proposal:** File an issue in `DarojaAI/.github` with `[RFC]` prefix.

**Discussion:** Team leads review in the issue thread (48-72 hour window).

**Decision:** 
- **Approved** → Issue converted to task, implementation scheduled
- **Deferred** → Revisit in next quarter
- **Rejected** → Documented reasoning, closed

**Implementation:** Changes merged to `.github`, then rolled out to repos per adoption plan.

### Security Issues

**Critical:** Report privately to infrastructure team (no public issues).

**Non-critical:** File issue in affected repo, tag `security`.

## Compliance & Auditing

### Monthly Check

1. Review branch protection status across all repos
2. Audit pre-commit hook versions (update if critical patches)
3. Check for secrets in recent commits (Gitleaks)
4. Verify CI/CD pass rates

### Quarterly Review

1. Update `.github` standards if needed
2. Review adoption status per repo (docs/ADOPTION-STATUS.md)
3. Identify lagging repos, schedule catch-up
4. Update this document

### Annual Audit

1. Full security scan (trivy, checkov, snyk)
2. Dependency audit (outdated packages)
3. Cost review (GitHub Actions, artifact storage)
4. Archive or sunset stale repos

## Roles & Responsibilities

### Repo Owner

- Maintain `CONTRIBUTING.md` (repo-specific deviations)
- Triage PRs and issues
- Enforce standards (pre-commit, branch protection)
- Update version/changelog on releases

### Infrastructure Lead

- Maintain `.github` templates
- Audit standards compliance
- Support repo owners on adoption
- Escalate security issues

### Maintainer (any committer)

- Review PRs (≥1 approval required)
- Run tests before merge
- Update docs if code changes
- Respond to issues within 48 hours

## Code Review Standards

**Required for merge:**
- 1 approval from maintainer
- All CI/CD checks passing
- Pre-commit hooks passing

**Best practices:**
- Review within 24 hours if flagged urgent
- Provide constructive feedback
- Test locally if complex changes
- Request changes if:
  - Missing tests
  - Breaking changes without migration path
  - Security concerns
  - Performance regression

## Release Process

1. **Version bump** in source (pyproject.toml / package.json / etc.)
2. **Create PR** with changelog entry
3. **Merge to main** after approval
4. **GitHub Actions auto-tags** with version
5. **Release notes auto-generated** from commit messages

See [docs/VERSIONING.md](./docs/VERSIONING.md) for details.

## Questions?

- **Standards questions:** Open issue in `.github`
- **Repo-specific:** Contact repo owner
- **Security:** Email infrastructure team privately
- **Governance:** Discuss in org meetings

---

**Last updated:** 2026-04-28
**Next review:** 2026-07-28 (Q3)
