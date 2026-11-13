# Baseline dev tooling

Do these one at a time. Skip any that don't apply and say why. Report findings before fixing.

| Tool | What to do | Notes |
| --- | --- | --- |
| **Gitleaks** (github.com/gitleaks/gitleaks) | Install it, run `gitleaks git -v` on the full history and `gitleaks dir -v .` on the working tree. Show every finding before doing anything else and say which provider each real key belongs to so the user can rotate it. Then add it as a pre-commit hook. | Don't rotate keys yourself. Mask secret values in output. |
| **Claude Code Security Review** (github.com/anthropics/claude-code-security-review) | Add its workflow as `.github/workflows/security-review.yml`, reading the key from a `CLAUDE_API_KEY` repo secret. If the repo is public, remind the user to require approval for workflows from outside contributors. | Pin actions to full commit SHAs. User creates the secret. |
| **Chrome DevTools MCP** (github.com/ChromeDevTools/chrome-devtools-mcp) | Run `claude mcp add chrome-devtools --scope user npx chrome-devtools-mcp@latest`. After the user restarts Claude Code, open the dev server in it and report console errors and failed network requests. | Needs a restart before the tools appear. |
| **Claude Code Action** (github.com/anthropics/claude-code-action) | Tell the user to type `/install-github-app` in Claude Code. | Requires repo admin; you can't do it for them. |
| **Trivy** (github.com/aquasecurity/trivy) | Install with brew or from the official releases page. **Never v0.69.4.** Run `trivy fs .` and report the HIGH and CRITICAL findings with the version that fixes each. | Check the release you install isn't the excluded version. |
| **Umami** (github.com/umami-software/umami) | Plan it as a service on the user's server (e.g. Coolify), or Umami Cloud if they don't want a server. Remind them the default login is `admin` / `umami` and must be changed first. When they give the website ID, add the tracking script to the root layout. | Don't run server steps from the repo; write them out. Cloudflare Web Analytics is the cookieless default if full analytics aren't needed. |
| **ccusage** (github.com/ccusage/ccusage) | Nothing to install. Run `npx ccusage@latest daily` and show the table. | |
| **Semgrep** (github.com/semgrep/semgrep) | Install with `pipx install semgrep` or `uv tool install semgrep`, run `semgrep scan --config auto`, report findings grouped by file. Don't fix anything yet. | |
| **ZAP** (github.com/zaproxy/zaproxy) | With the dev server running: `docker run -t ghcr.io/zaproxy/zaproxy:stable zap-baseline.py -t http://host.docker.internal:[port]`. Report each warning and what the fix would change. | Only against the user's own dev/test server. |
| **Drizzle** (github.com/drizzle-team/drizzle-orm) | **Only if the project uses a SQL database with no ORM.** Install `drizzle-orm`, `drizzle-kit` (dev) and the driver for the database, write `drizzle.config.ts`, pull the existing schema with `drizzle-kit pull`. | Never run a migration against anything but the local database. Don't layer a second schema over tables owned by a CMS. |
| **React Bits** (github.com/DavidHDev/react-bits) | **Only if the app is React.** Add `"@react-bits": "https://reactbits.dev/r/{name}.json"` under `registries` in `components.json` so components install with `npx shadcn@latest add @react-bits/<Name>`. | Add no components until asked; any added must respect reduced motion. |
| **Marketing Skills** (github.com/coreyhaines31/marketingskills) | Run `npx skills add coreyhaines31/marketingskills`. | |
| **OWASP Cheat Sheets** (cheatsheetseries.owasp.org) | Nothing to install. Add this line to CLAUDE.md: "Before writing code for auth, sessions, passwords, file uploads, input validation or user data, read the matching OWASP cheat sheet at cheatsheetseries.owasp.org and follow it." | Included in `claude-md-template.md`. |
| **GSD-Core** (github.com/open-gsd/gsd-core) | Install the skill to plan, phase, build and review the project in spec-driven development. | See `claude-setup.md`. |
| **SlopMonster** (github.com/ItsssssJack/SlopMonster) | Install so copywriting and all text on the site is checked and kept "anti-AI". | Run over public copy during UI/content reviews. |
| **Hallmark** (github.com/nutlope/hallmark) | Install to review designs and fix AI design flaws against best practice. | Use for greenfield pages, redesigns and audits. |

## Pre-commit and CI conventions

- Keep hooks fast: gitleaks on staged changes only; heavier scans (Trivy, Semgrep, ZAP) belong in CI or on demand.
- CI: run unit tests and these scans in GitHub Actions on GitHub-hosted runners; pin third-party actions to full SHAs; enable Dependabot for `github-actions` and the package ecosystem.
- Don't run builds on shared production hosts.

## Final report table

| Tool | Installed / skipped | Where it lives | Waiting on user |
| --- | --- | --- | --- |
