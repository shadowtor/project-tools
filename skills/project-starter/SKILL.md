---
name: project-starter
description: Starter package for a new or existing project - picks the architecture and stack from opinionated defaults (Astro, Next.js, Postgres, Drizzle, Payload, Expo, Shopify, Docker/Coolify, Cloudflare), installs and runs the baseline dev tooling (Gitleaks, Trivy, Semgrep, ZAP, ccusage, Claude Code security review, Chrome DevTools MCP), sets up Claude with a CLAUDE.md template, plugins, MCP servers and skills, and creates a baseline-audit phase to review the current codebase. Use whenever the user starts, scopes, plans or onboards a project, says "new project", "starter stack", "base project", "set up my toolbelt", "what stack should I use", or asks to add the standard tools and checks to a repo.
---

# Project starter

Two jobs, usually run together:

1. **Choose the stack** for a new project, or record the existing one for an onboarded repo.
2. **Bring the repo up to baseline**: tooling, Claude setup, and a review phase over the current code.

The defaults live in `references/` so they can be edited without touching this file. The defaults are the author's preferences; where the user states a different preference, theirs wins. Record any override in the project's CLAUDE.md under "Stack decisions" so later sessions inherit it.

| Reference | Read it when |
| --- | --- |
| `references/stack.md` | choosing architecture, UI, database, auth, storage, email, hosting, deploy flow |
| `references/toolbelt.md` | a project needs a specific capability (forms, 3D, maps, tables, animation, perf testing) |
| `references/dev-tooling.md` | installing or running the baseline security/QA tools |
| `references/claude-setup.md` | setting up Claude Code plugins, MCP servers, skills and the GSD workflow |
| `references/claude-md-template.md` | creating or extending the project's CLAUDE.md |

## Workflow

### 1. Detect the situation
New project (empty or near-empty repo) or existing repo? Read the repo's README, package manifest and CLAUDE.md first; preserve a working project's identity and architecture rather than re-platforming it.

For an existing repo, build a codebase map first (`/graphify` if installed) so the later review phase works from the graph, not scattered file reads.

### 2. Stack (new projects, or record for existing)
Use the decision table in `references/stack.md`. Pick the smallest combination that meets the requirements - being in the toolbelt does not mean a project needs the tool. Verify current stable versions and supported integrations from official docs (or Context7 if available), not memory. Confirm the choice with the user in one short message (one recommendation, not a menu), then record it.

### 3. Claude setup
Follow `references/claude-setup.md`: add the CLAUDE.md template sections that apply, check which recommended plugins/MCP servers/skills are already present, and tell the user the exact install commands for the missing ones. Don't install plugins into the user's global config without asking. Ponytail is part of the core set; the "Reviewed skill packs" table lists which third-party packs to add for the project's stack (Postgres, Next.js, Expo, SEO, Playwright) - suggest only those that fit.

### 4. Baseline tooling
Follow `references/dev-tooling.md` item by item. Hard rules that apply to every tool:
- Pin every GitHub Action you add to a full commit SHA, with the version in a comment.
- If a command fails or the docs have changed, read the tool's README and tell the user before trying something else.
- Show findings before fixing anything; never rotate keys or touch production without asking.
- Analytics (Umami) belongs on a server, not in the repo: write the steps and don't run them.

### 5. Baseline review phase
Create a phase (a GSD phase if the project uses GSD, otherwise a `docs/baseline-audit.md` checklist) that runs, against the current codebase: the tool scans, plus the sibling skills from this repository where they apply - `security-holes-audit`, `app-legal-risk-audit`, `ux-details-builder`, and `claude-md-cleanup` - and ends with a fix/add/change list for the user to approve.

### 6. Report
Finish with a table: **tool, installed or skipped, where it lives, anything still waiting on the user** (accounts, secrets, restarts, admin rights).
