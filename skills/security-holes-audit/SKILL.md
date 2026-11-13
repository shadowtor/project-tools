---
name: security-holes-audit
description: Report-first security audit of a web or mobile app against two checklists of the holes that show up in almost every fast-built or AI-generated app - committed .env files, keys in the frontend, open Supabase/Firebase rules, client-side permission checks, missing rate limits, SQL injection, XSS, localStorage tokens, CORS *, IDOR, mass assignment, unsigned webhooks, plus infrastructure holes like a served .git folder, price trusted from the browser, weak JWT secrets, race conditions, dangling DNS, SSRF, open redirects, missing framing protection and email-enumeration. Use whenever the user asks for a security review, "check my app for holes", "vibe-coded security", pre-launch hardening, or pastes a numbered list of security issues - even if they don't say "audit".
---

# Security holes audit

Checks a codebase against two lists, reports on every item first, and only changes code the user approves. The point of report-first is that several fixes touch auth, keys or production, where a wrong automatic change does more damage than the hole.

| List | File | Items |
| --- | --- | --- |
| Application-level holes (vibe-coded apps) | `references/application-holes.md` | 20 |
| Platform, infra and logic holes | `references/platform-and-logic-holes.md` | 20 |

Run both by default. If the user pastes their own numbered list, use theirs and treat these files as extra "how to check" guidance.

## Rules of engagement

- **Report all items before fixing any.** No edits during the report phase.
- **Don't** install packages, change or rotate any key, or touch production without asking first. Say which keys need rotating and why, and let the user do it.
- Some items are DNS, hosting, or provider settings, not code. For those, give the exact setting to check (where to click, what value is safe) and mark **USER ACTION**; don't guess at the answer.
- After the report, fix only approved items, **one at a time**, run the project's own checks (tests, lint, typecheck, build) after each, and show the diff.
- Never paste a discovered secret into chat; give the file, line and provider, and mask the value.

## How to run it

1. **Orient.** Detect framework and version (Next.js middleware-only auth is version-sensitive), auth provider, database/BaaS, hosting, payment/webhook providers, AI features, and whether it's web, mobile or both.
2. **Per item**, decide: **APPLIES / DOESN'T APPLY / CAN'T TELL (USER ACTION)**, and say how you can tell (file and line, config key, or "no such feature in this app"). Use the "How to check" column; read the code, don't rely on a grep hit.
3. **For APPLIES:** state the severity (critical / high / medium / low), the exact fix, and what the fix would change (files, behaviour, any migration or key rotation).
4. **Output** a table `# | item | verdict | evidence (file:line) | severity | fix`, a short fix order (secrets and open data first, then auth and money paths, then hardening), and the list of USER ACTION items. Ask which to fix.
5. **Fix phase** as per the rules above.

Useful quick checks that cover several items: `git log -p -S` for secrets in history (or Gitleaks if installed), response headers from the deployed site (framing, CORS), a logged-out request against every storage URL and API route, and a second test account against every owner-scoped resource.

For a broader whole-codebase pentest-style review beyond these checklists, use a dedicated security-audit workflow; this skill is the fast, checklist-driven pass.
