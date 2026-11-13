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
| Graphify | github.com/Graphify-Labs/graphify (Apache-2.0, `/graphify`) | Turn a codebase, docs, schemas and PDFs into a queryable knowledge graph with local AST parsing; run it first when onboarding an existing repo |
| security-audit | local skill | Full multi-phase security audit (heavy; offered at release gates) |
| project-stack | superseded by `project-starter` | Stack defaults |
| open-source-standards | based on opensource.guide | Community files, licensing, security settings for public repos |
| marketingskills | github.com/coreyhaines31/marketingskills | Marketing skill pack |
| This repo's skills | `app-legal-risk-audit`, `security-holes-audit`, `ux-details-builder`, `claude-md-cleanup`, `project-starter` | Audits and builders |

## Reviewed skill packs

Reviewed 2026-11-13. Install with `npx skills add <owner/repo>` (or the pack's own command) and pick the skills listed; don't vendor them into this repo, so they stay current upstream. Check the licence before copying any pack's files.

| Pack | Verdict | Take these | Fits |
| --- | --- | --- | --- |
| supabase/agent-skills -> `supabase-postgres-best-practices` | **Add** (works on any Postgres, not just Supabase) | the one skill | Every PostgreSQL + Drizzle project: schema, indexes, RLS, query design |
| addyosmani/web-quality-skills | **Add the 2 missing**, rest already installed | `performance`, `seo` (already have `accessibility`, `best-practices`, `core-web-vitals`, `web-quality-audit`) | Website launch gate and `test.<domain>` audits |
| SNLabat/SEO-GEO-AEO-Skill | **Add (optional)**; repo has no licence, so install by reference only | `SKILL.md` audit | SEO plus AI-search (GEO/AEO) audit at launch; complements `seo` above |
| playwright.dev/agent-cli/skills | **Add** | `playwright-cli install --skills` (-g for global) | Test generation and healing, tracing, request mocking, storage state; complements the Playwright plugin for the per-phase smoke E2E |
| vercel-labs/agent-skills | **Add selectively** | `composition-patterns` (React/shadcn component APIs), `react-native-skills` (Expo), `react-view-transitions` (pairs with UX detail 9); already have `react-best-practices`, `web-design-guidelines` | Next.js and Expo projects. Skip `deploy-to-vercel`, `vercel-cli-with-tokens`, `vercel-optimize` (stack deploys via GitHub Actions, GHCR and Coolify, not Vercel) and `writing-guidelines` (SlopMonster covers copy) |
| emilkowalski/skills | **Add selectively** (optional polish layer) | `emil-design-eng`, `animate`, `animation-performance`, `gestures`, `apple-design`, `break-ui`, `review-animations`, `learn-taste` | Frontend polish. Overlaps `make-interfaces-feel-better` and `userinterface-wiki`; `gestures` and `apple-design` back `ux-details-builder` items 10-11. Pick one primary motion advisor per project |
| shadowtor/BrandDesign (`brand-studio`) | **Already have** | whole skill | Branding step for new projects: logo, palette, favicons, social, guidelines PDF |
| DietrichGebert/ponytail | **Core** (already in plugins) | `ponytail`, `ponytail-review`, `ponytail-audit`, `ponytail-debt` | Keep solutions minimal; set mode in CLAUDE.md template |
| Graphify-Labs/graphify | **Already have; keep** | `/graphify` | Onboarding existing repos |
| collectivebrain.de `design-design-critique`, `design-research-synthesis` | **Skip** - same Anthropic originals as the `design` plugin's `design-critique` and `research-synthesis` | none | Already covered |
| collectivebrain.de `github-actions-sparen` | **Skip as install** - the minutes/billing guidance is already in `stack.md` hosting rules and the local `github-workflows` skill; revisit if a deploy is blocked by a spending limit | none | Reference only |

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
