# Local CLI System

Last refreshed: 2026-06-27.

This document captures Kevin's current AI coding CLI operating model. It is a public, sanitized snapshot: do not store tokens, API keys, OAuth secrets, browser cookies, machine-local private paths, or private project context here.

## Operating Model

| Area | Default |
| --- | --- |
| Primary agent CLI | `codex` |
| Compatibility agent CLIs | `claude`, `gemini`, `qwen` |
| Source control | `git`, `gh` |
| Cloud platforms | Provider CLIs, not provider MCPs |
| Browser QA | Playwright CLI, with MCP only when an agent-native browser surface helps |
| Documentation lookup | `context7` MCP, not broad web scraping |

Principle: deterministic cloud, database, source-control, and deployment operations should use CLIs. MCP should stay small and reserved for agent-native value.

## Decision Framework

Use this framework when deciding whether a capability belongs in a CLI, a script, an MCP server, or a skill.

Canonical skill: [`cli-tooling-governance`](https://github.com/kt-aicoding/skills/blob/main/skills/codex/cli-tooling-governance/SKILL.md).

| Capability shape | Best home | Reason |
| --- | --- | --- |
| Repeatable command sequence with clear inputs and outputs | CLI tool | Easy to test, compose, document, and run outside an agent |
| Project-local one-off automation | Project script | Keeps context and assumptions local to the project |
| Agent behavior, review checklist, or operating procedure | Skill | The value is instruction and judgment rather than executable code |
| Agent-native access to external context or browser state | MCP server | The value is a live tool surface inside the agent loop |
| Cloud deployment, billing/resource inspection, env management | Provider CLI | Mature CLIs are more deterministic and easier to audit |

Default to the smallest surface that solves the problem. Promote only when repetition, users, and maintenance cost justify it.

## CLI Layers

| Layer | Tools | Role |
| --- | --- | --- |
| Agent entrypoints | `codex`, `claude`, `gemini`, `qwen` | AI coding sessions, compatibility, and cross-checks |
| Cloud and deploy | `vercel`, `supabase`, `wrangler`, `tcb`, `netlify`, `flyctl`, `railway`, `eas` | Deployments, resources, login state, cost-risk discovery |
| JavaScript and TypeScript | `node`, `npm`, `pnpm`, `bun`, `tsx`, `tsc` | Web app and Node tooling |
| Python | `uv`, `pipx`, `python3` | Python projects and utility scripts |
| Go and Java | `go`, `mvn`, `java` | Backend and library projects |
| QA and security | `playwright`, `actionlint`, `gitleaks`, `osv-scanner` | Browser checks, GitHub Actions linting, secret/dependency risk scans |
| Registry and packaging | `clawhub`, `openclaw`, `mcporter` | Skill/package and MCP-related maintenance |

## Current Provider CLI State

| Provider | CLI state | Resource note |
| --- | --- | --- |
| Vercel | logged in | Hobby team; multiple projects and several Cron jobs visible; no contracts, Blob stores, Marketplace resources, or alert groups found from CLI |
| Supabase | logged in | demo apps consolidated into one shared project |
| CloudBase | logged in | one normal personal environment visible |
| Cloudflare | logged in through `wrangler` | real resources exist: Pages, KV, D1, R2, and Queue |
| Netlify | logged in | current account site list returned empty |
| Fly.io | logged in | app list returned empty |
| Railway | installed, not authenticated | complete `railway login --browserless` before resource or cost checks |
| GitHub | local keyring auth works | use `env -u GH_TOKEN gh ...` if an outer shell has a stale `GH_TOKEN` |

## Repository Placement

| Content | Preferred repository |
| --- | --- |
| CLI utilities and CLI operating docs | `kt-aicoding/cli-tools` |
| MCP server staging and MCP operating docs | `kt-aicoding/mcp-servers` |
| Canonical index across skills, MCP, CLI, workflows | `kt-aicoding/registry` |
| Reusable skills | `kt-aicoding/skills` |
| Claude Code and Codex configuration kit | `kt-aicoding/claudecode-codex-config` |
| Agent workflow templates | `kt-aicoding/agent-workflows` |

## Maintenance Commands

Run before cloud operations:

```bash
vercel whoami
supabase projects list
wrangler whoami
tcb env list
netlify status
flyctl auth whoami
railway whoami
```

Run for monthly tool health:

```bash
codex mcp list
codex doctor --json
npm outdated -g --depth=0
env -u GH_TOKEN gh auth status --active
actionlint --version
gitleaks version
osv-scanner --version
```

Run for monthly platform resource checks:

```bash
vercel projects list --scope kevintens-projects
supabase projects list
wrangler pages project list
wrangler kv namespace list
wrangler d1 list
wrangler r2 bucket list
wrangler queues list
tcb env list
netlify sites:list --json
flyctl apps list --json
```

## Governance Notes

- Prefer provider CLIs for cloud operations: `vercel`, `supabase`, `wrangler`, `tcb`, `netlify`, `flyctl`, `railway`.
- Prefer `gh` for GitHub. Avoid default-enabling a GitHub MCP when local keyring auth works.
- Prefer project-local tools and wrappers over global installations when a project provides them.
- Do not install Docker, Colima, Flutter, Dart, or global Gradle until a concrete project requires them.
- Treat Cloudflare as a real cost surface when R2/D1/Queues resources exist.
- Keep Supabase demo development in the existing shared project unless a separate project is explicitly required.
- Upgrade global tools deliberately: patch-level agent CLI updates are low risk; `pnpm` major upgrades and Railway major upgrades should be project-driven.

## Anti-Patterns

| Anti-pattern | Why it is risky | Preferred approach |
| --- | --- | --- |
| Wrapping every provider CLI in custom scripts | Hides provider behavior and makes failures harder to debug | Use provider CLI directly until repetition is proven |
| Enabling provider MCPs by default | Adds auth/session complexity and duplicates CLI capability | Keep provider MCPs task-specific |
| Global upgrades without a project need | Can break lockfiles, CLIs, or local build assumptions | Upgrade during maintenance windows with verification |
| Storing machine-local state in public docs | Leaks private assumptions and becomes stale quickly | Keep public docs sanitized and keep private inventory local |
| Building a tool before writing the command sequence | Premature abstraction | Document the manual sequence first, then automate |

## Upgrade Policy

| Tool class | Policy |
| --- | --- |
| Agent CLI patch updates | Low risk; upgrade during maintenance windows and verify `codex doctor --json` plus `codex mcp list` |
| Package manager major versions | Do not auto-upgrade; major changes can affect lockfiles and project scripts |
| Provider CLIs | Monthly version check; upgrade only when required for a task or security fix |
| Railway CLI | Upgrade and authenticate only when Railway resource/cost checks or deployments are needed |

## Reference Checklist

Before adding a new CLI tool to this organization:

- It has a specific AI coding workflow purpose.
- It has a clear owner repository and does not duplicate an existing mature CLI.
- It has a documented command surface, at least `--help` or equivalent.
- It does not require committed secrets, private paths, or personal machine state.
- It can be verified with a small deterministic command.
- It has a promotion path: docs -> script -> CLI -> standalone repository.
