# Claude toolbelt

The author's working setup, grouped so a newcomer can start with the core and add the rest on demand. Check what is already installed before suggesting anything (look at the user's `~/.claude/settings.json` `enabledPlugins`, `claude mcp list`, and `~/.claude/skills`). Install commands are in the Claude Code CLI: `/plugin marketplace add <owner/repo>` then `/plugin install <plugin>@<marketplace>`.

## Core plugins

| Plugin | Marketplace (repo) | Why |
| --- | --- | --- |
| superpowers | claude-plugins-official (`anthropics/claude-plugins-official`) | Process skills: brainstorming, plans, TDD, debugging, verification |
| skill-creator | claude-plugins-official | Build and share skills like the ones in this repo |
| context7 | claude-plugins-official | Current library docs instead of stale memory |
| github | claude-plugins-official | PRs, issues, repo operations |
| feature-dev | claude-plugins-official | Explorer/architect/reviewer agents for features |
| code-review | claude-plugins-official | Review a diff or PR |
| security-guidance | claude-plugins-official | Security reminders while editing |
| frontend-design | claude-plugins-official | Distinctive, non-generic UI |
| playwright | claude-plugins-official | Browser automation and E2E |
| chrome-devtools-mcp | claude-plugins-official | Console, network, performance from a real browser |
| claude-code-setup | claude-plugins-official | Recommends automations for a codebase |
| everything-claude-code | `affaan-m/everything-claude-code` | Large library of language reviewers, build resolvers, patterns |
| ponytail | `DietrichGebert/ponytail` | Keeps solutions minimal; audits over-engineering |

## Situational plugins

| Plugin | Use when |
| --- | --- |
| stripe | Payments/billing work |
| liquid-skills | Shopify Liquid themes |
| icons (`FortAwesome/fontawesome-agent-tools`) | Font Awesome icon workflow |
| open-design (`feimacode/open-design-agent-kit`) | Design artifacts, critique, decks |
| solidcraft-studio (`shadowtor/solidcraft-studio`) | CAD/3D-print design work |
| engineering-skills, engineering-advanced-skills, marketing-skills | Broad engineering and marketing skill packs |
| cloudflare (`cloudflare/skills`) | Cloudflare Workers/product work (off by default; enable per project) |
| discord | Discord channel integration |

## Standalone skills

| Skill | Source | Purpose |
| --- | --- | --- |
| GSD (get-shit-done / gsd-core) | github.com/open-gsd/gsd-core | Spec-driven planning, phases, execution, review, verification |
| SlopMonster | github.com/ItsssssJack/SlopMonster | Remove AI writing tells from public copy |
| Hallmark | github.com/nutlope/hallmark | Anti-AI-slop design review |
| graphify | `/graphify` | Turn any input into a knowledge graph |
| security-audit | local skill | Full multi-phase security audit (heavy; offered at release gates) |
| project-stack | superseded by `project-starter` | Stack defaults |
| open-source-standards | based on opensource.guide | Community files, licensing, security settings for public repos |
| marketingskills | github.com/coreyhaines31/marketingskills | Marketing skill pack |
| This repo's skills | `app-legal-risk-audit`, `security-holes-audit`, `ux-details-builder`, `claude-md-cleanup`, `project-starter` | Audits and builders |

## MCP servers

| Server | Purpose | Notes |
| --- | --- | --- |
| context7 | Library docs | Core |
| chrome-devtools | Browser inspection | `claude mcp add chrome-devtools --scope user npx chrome-devtools-mcp@latest` |
| playwright | Browser automation | Via plugin |
| coolify | Operate a self-hosted Coolify (deploys, logs, env vars) | Needs the user's Coolify URL and token; keep tokens out of the repo |
| netdata | Infra metrics and alerts | Optional, for self-hosted infra |
| blender | 3D modelling via Blender | Optional, for 3D projects |

Never commit MCP tokens; use user-scope config or environment variables.

## GSD workflow settings worth copying

Spec-driven development with research, plan-check, verifier and code-review steps on, `auto_advance` off, `commit_docs` on, parallelization on. Per-agent model overrides are optional (see the model-allocation section in `claude-md-template.md`). Phase 1 of a new web project = infrastructure + coming-soon page; feature phases target the `test` branch.

## Everything here is a starting point

The user will add and adjust over time. When they name a new tool or preference, add a row to the right table above (and a line to `stack.md` or `dev-tooling.md` if it affects project setup) rather than creating a new file.
