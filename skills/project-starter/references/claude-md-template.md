# CLAUDE.md template

Add the sections that apply; skip the rest. Keep it short - run `claude-md-cleanup` afterwards. Each block is a preference of the author; adjust to the user's stated ones.

## Editing discipline
- Before editing any file, read it first. Before modifying a function, grep for all callers. Research before you edit.
- Do not read or search `node_modules`, `.git`, `dist`, `__pycache__` unless asked.

## Verification
- Before asking a human to verify behaviour that a browser tool (Playwright, Chrome DevTools MCP) can observe, drive the tool first and check whether the problem is reproducible. Escalate to a human only for things automation can't cover (physical device, third-party auth flow, subjective taste).

## Node
- Default to the latest Node LTS. Pin `.nvmrc`, `package.json` `engines`, the Dockerfile base image and CI to it, and run local verification under it. Fall back to the previous LTS only when a dependency or the host demonstrably blocks the latest (verify with `npm view <pkg> engines.node` and a real install/build/test), and record the blocker in the project docs.

## Minimal solutions (Ponytail, with GSD)
- Ponytail runs in `full` mode for the whole project and is applied at every GSD stage, not only when writing code:
  - **Plan / discuss / spec:** apply the ladder to each requirement and task - does it need to exist, is it already in the codebase, does the stdlib or a native feature cover it, is an installed dependency enough. Cut or shrink tasks that fail; record "skipped: X, add when Y" in the plan instead of building speculative scope. The planner and plan-checker must not add abstractions, config or files the requirements don't need.
  - **Execute:** before writing code, grep for existing helpers and all callers of anything being changed; fix bugs at the root cause. Executors and sub-agents get this instruction in their prompt.
  - **Review / verify:** run `/ponytail-review` on each phase's diff and `/ponytail-audit` at milestone completion; report what to delete or simplify. Ponytail simplifies how it's built, it never trims the phase's stated requirements or acceptance criteria.
  - Non-trivial logic leaves one small runnable check behind. Never simplify away input validation, security measures, error handling that prevents data loss, or accessibility.
  - Log accepted shortcuts as `ponytail:` comments naming the ceiling and upgrade path; use `/ponytail-debt` to list them.

## Codebase map (Graphify, with GSD)
- On an existing repo, run `/graphify` first and query `graphify-out/` before broad file searches.
- **Exclude dot-files and dot-folders** from the graph: `.env*`, `.planning/`, `.git/`, `.claude/`, and other hidden config. Secrets must never be ingested, and GSD's planning state is read by GSD directly, not through the graph. Graphify honours `.gitignore`, `--exclude` patterns and a project `.graphifyignore`; put these patterns in `.graphifyignore` (and add `graphify-out/` to `.gitignore` if the graph isn't meant to be committed), then check the "skipped by ignore rules" summary after extraction to confirm nothing hidden was read.
- Rebuild the graph after a phase changes structure so GSD researchers and mappers query a current map; GSD artifacts remain the source of truth for requirements, plans and state.

## Security
- Before writing code for auth, sessions, passwords, file uploads, input validation or user data, read the matching OWASP cheat sheet at cheatsheetseries.owasp.org and follow it.
- Per phase: review the touched surface (auth, input handling, data access, secrets, uploads, webhooks, CORS/headers, deploy config); report findings with evidence or "no issues found in touched surface". At release gates (opening a PR, milestone completion, pre-production deploy) offer the full security audit.
- Pin every GitHub Action to a full commit SHA with the version in a comment.

## Testing scope
- Per phase: keep E2E narrow - a smoke pass over the main pages plus E2E only for flows the phase touched. Unit tests for logic stay as they are.
- Run the full E2E suite at release gates (PR open/finalise, milestone completion, pre-production deploy).
- If a phase breaks something outside its scope, add the covering E2E test then.
- E2E runs against the deployed `test.<domain>` environment, not a local dev server; unit tests run locally. If the test deploy is stale or unreachable, fix it rather than falling back to local and calling it verified.

## Copy review
- Whenever text or frontend is written or reviewed, run SlopMonster over the public-facing copy; report the score and findings; never invent facts, numbers or testimonials to satisfy the checker.

## Stack decisions
- Follow the `project-starter` stack defaults unless overridden here. Record overrides and justified exceptions below:
  - (none yet)

## Model allocation (optional, Claude GSD)
- Routine planning, research, implementation, docs: the mid-tier model. Bounded mapping/classification: the small model. Escalate to the top-tier model only for material uncertainty, significant architecture tradeoffs, conflicting evidence, complex security judgment, or a repeatedly failing check - not for access or infrastructure failures. Return to the mid-tier model once the decision is made. Pass the model explicitly when spawning sub-agents.
