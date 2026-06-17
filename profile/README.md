# DarojaAI

> A small GitHub org building a stack from bare cloud VM to Discord AI agent.

DarojaAI is a 42-repo organization with three layered concerns:

- **Provisioning** — bare VM to Discord AI agent (Terraform → Linux setup → desktop seed → gateway)
- **Intelligence** — knowledge (`dev-nexus`), capability (`mcp-tooling`), coordination (`openclaw-gateway`)
- **Products** — end-user-facing applications on top

Heavily GCP-flavored. Governed by a [P0 contract](https://github.com/DarojaAI/dev-nexus/blob/main/docs/architecture/architectural-boundaries.md) that defines what each repo owns.

## Where to start

| If you want to… | Go to |
|---|---|
| Understand how the org is structured | [GOVERNANCE.md](https://github.com/DarojaAI/.github/blob/main/GOVERNANCE.md) |
| Contribute to any repo | [CONTRIBUTING.md](https://github.com/DarojaAI/.github/blob/main/CONTRIBUTING.md) |
| Read CI/CD standards | [docs/CI-CD-STANDARDS.md](https://github.com/DarojaAI/.github/blob/main/docs/CI-CD-STANDARDS.md) |
| See per-repo adoption status | [docs/ADOPTION-STATUS.md](https://github.com/DarojaAI/.github/blob/main/docs/ADOPTION-STATUS.md) |
| Start an agent session | [docs/AGENTS-TEMPLATE.md](https://github.com/DarojaAI/.github/blob/main/docs/AGENTS-TEMPLATE.md) |

## Key repos

- **`dev-nexus`** — pattern discovery & institutional memory, owns the P0 boundary contract
- **`mcp-tooling`** — external API integrations (vm-ops, Duffel, Cal.com, payments)
- **`openclaw-gateway`** — Discord agent runtime
- **`infra-actions`** — composite GitHub Actions, used by ~73% of repos with workflows

## Architecture at a glance

```
L1: terraform-hcloud-linux-vm        ← bare VM
L2: linux-{headless,desktop}-setup   ← OS + dev env
L3a: linux-desktop-seed              ← deploy orchestration
L3b: openclaw-gateway                ← Discord agent runtime
```

On top of L1–L3b, the intelligence triad and products layer in. See [`darojaai_architect/ARCHITECTURE.md`](https://github.com/DarojaAI/darojaai_architect/blob/main/ARCHITECTURE.md) for the full map.

## License

Most repos are GPL-3.0. Some consumer apps are PROPRIETARY (Milan Patel). Each repo's `LICENSE` file is authoritative.

---

*This README renders on the [DarojaAI GitHub org page](https://github.com/DarojaAI). Source: `DarojaAI/.github/profile/README.md`.*
