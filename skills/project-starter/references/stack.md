# Stack defaults

Standing defaults unless the user overrides them or a documented requirement makes them unsuitable. Don't re-research settled choices. Choose the smallest combination that meets the requirements. Prefer maintainable, mostly free, open-source and self-hostable software; Cloudflare and Shopify are intentional hosted choices. Free-to-use, source-available and open-source are not interchangeable: check licences and plan limits for the actual use (n8n and Directus especially).

Preserve an existing project's architecture. Record the selected stack, exceptions and service ownership in the project's CLAUDE.md / planning docs so planners and executors inherit them.

## Architecture decision table

| Requirement | Starting stack | Boundary |
| --- | --- | --- |
| Marketing, business, portfolio, blog, docs, community site | Astro | Add a CMS only when someone needs an editing interface |
| Site with client-editable content | Astro + EmDash | EmDash runs inside the Astro app; confirm supported DB, adapter and media storage |
| Custom SaaS, portal, dashboard, authenticated app | Next.js + PostgreSQL + Drizzle | Add auth and services only as needed |
| App with substantial content/admin/backend | Next.js + Payload + PostgreSQL | Payload owns its collections, permissions, APIs, migrations |
| Simple SaaS needing a bundle of backend services fast | Next.js + Supabase | When auth, storage, realtime and generated APIs justify it |
| Cross-platform iOS/Android (optionally with web) | Expo + React Native + TypeScript | Reuse the selected backend; EAS is optional |
| Mobile with a justified exception | Flutter, or native Swift/Kotlin | Only for a documented reason (existing codebase, deep platform needs) |
| Internal/data-heavy app | Next.js + Mantine, or Directus | Directus when a DB admin UI + APIs is enough |
| Ecommerce | Shopify; Hydrogen + Oxygen for custom headless | Keep products, checkout, payments, orders in Shopify |
| Existing WordPress | WordPress (+ WooCommerce if needed) | Preserve existing investment and plugins |
| Bot, worker, automation, service with no frontend | Appropriate runtime + Docker | Skip website launch rules |

## Core technologies

| Technology | Purpose | Notes |
| --- | --- | --- |
| Astro | Content-focused sites | Islands where interactivity is needed |
| EmDash | Astro CMS | Preferred when an Astro site needs editable collections |
| Next.js | Web apps | TypeScript for custom app code |
| Payload | App CMS/backend | Don't add to a simple site with no backend need |
| PostgreSQL | Relational data | Private networking, versioned migrations; packaged services use their supported engines |
| Drizzle ORM | Type-safe SQL + schema | For app-owned schema; never layer a Drizzle schema over CMS-managed tables |
| Docker + Coolify | Packaging, deployment | Default for self-hosted apps, DBs, workers |
| Cloudflare | DNS, CDN, edge, tunnels | Tunnels for web ingress, Access for restricted UIs, Turnstile for public forms |

## UI and motion

Choose **one** component system per frontend:
- Tailwind + daisyUI: default for Astro and straightforward sites.
- Tailwind + shadcn/ui + Kibo UI: custom Next.js products.
- Mantine: dashboards, admin panels, data-heavy internal tools.

Reskin through theme tokens and CSS variables first. Charts: Chart.js (react-chartjs-2 in React) with accessible labels and a text/table equivalent. Motion: GSAP or Anime.js, not both; Motion or react-spring only for a concrete need. Respect `prefers-reduced-motion`, protect LCP/CLS, don't add an animation library when CSS suffices. Specialist libraries: see `toolbelt.md`.

## Mobile

Expo + React Native + TypeScript with Expo Router; NativeWind and React Native Reusables optional. EAS Build/Submit/Update are optional conveniences - keep a documented build path that works without paid Expo services, prefer local builds or self-hosted CI, and don't promise "free" (store accounts, Mac/macOS runner for iOS, backend hosting all cost money). Share one backend, auth authority and data model between web and mobile; keep platform UI separate; native clients can't rely on browser cookies or Next.js server actions alone.

Decision tree: content/search-driven -> Astro; account areas without native needs -> Next.js (PWA if enough); app-store, repeated use, offline, device features -> Expo; both -> Next.js + Expo sharing the backend; Flutter/native only for documented reasons.

## Optional supporting tools (add only for a named requirement)

| Tool | Use |
| --- | --- |
| n8n | Workflow automation across APIs/webhooks |
| Supabase / Appwrite | BaaS (pick at most one; use its auth rather than adding Better Auth) |
| Directus | Staff admin over structured data |
| Better Auth | Custom TypeScript app auth when the backend has none |
| Authentik | Central SSO across several services |
| Cloudflare R2 | Hot, frequently served files (zero egress); keep restricted files private and serve via signed URLs or an authenticated route |
| Backblaze B2 | Cold/bulk storage: backups, archives, uploads, exports; scoped keys; immutable backups; check data-residency before storing personal data offshore |
| Redis | Cache, queues, rate limiting, locks; Postgres stays authoritative |
| Cloudflare Web Analytics | Default cookieless analytics (no consent banner needed); off on `test.` |
| Umami | Full analytics when needed (self-hosted or cloud) |
| Uptime Kuma | Availability monitoring, ideally outside the app's failure domain |
| GlitchTip or PostHog | Error tracking - pick one primary; PostHog adds replay/product context |
| Listmonk + Postal | Newsletters (Listmonk) and delivery (Postal); double opt-in, unsubscribe, bounce handling |
| Mailpit | Mail catcher for local and `test` environments |
| Fail2Ban (+ Fail2Ban UI) | Log-based bans on Linux hosts you administer |

## Avoid overlapping responsibilities

- One CMS/backend per set of models; don't stack EmDash, Payload and Directus over the same data.
- One migration owner per schema; one session/auth authority (SSO and Cloudflare Access don't replace app authorization).
- One storage owner per file flow; split R2 vs B2 by access pattern, not by project.
- Uptime checks reachability; error tracking captures exceptions; don't collect exceptions twice by default.

## Email

Mail transport is chosen by environment variables only (`SMTP_HOST/PORT/USER/PASS/SECURE`, `MAIL_FROM`); code never branches on environment. Local and `test` send to Mailpit and never receive production credentials; production uses the real SMTP service. Mailpit can't prove deliverability: before launch, configure SPF, DKIM, DMARC, return-path and PTR, send to an owned inbox, and confirm bounces are handled.

## Hosting and deploy

- Build in GitHub-hosted Actions, push an image to GHCR (commit-SHA tag + moving branch tag), and have the host pull the finished image; deploy only after verification and publishing succeed. Keep builds off shared production servers.
- Design for the free allowance (public repos free; private repos share plan minutes); cache, cancel superseded runs, short artifact retention, no surprise overages.
- Build-time public variables as build args; runtime secrets in the host's secret store; never in the repo or the frontend bundle. Pin third-party actions and reusable workflows to full commit SHAs, with Dependabot for `github-actions`.
- Default-deny inbound on new Linux hosts; SSH keys only, no password or root login; verify real exposure (Docker-published ports can bypass host firewalls); keep databases and queues on private networks; protect admin UIs with SSO/Access.
- Enable security updates; set up swap on every host.

## Database and persistence

PostgreSQL for custom app data. Versioned migrations, foreign keys, constraints and `NOT NULL` over validation-only rules. Created/updated timestamps on app-owned records and an audit trail for sensitive changes. Scheduled backups with tested restores, including volumes and service config. A backup carries every category of personal data the database holds, so record where it's stored and confirm that's acceptable; encrypt with a key held separately where it isn't. Separate credentials per service; never expose a database publicly.

## Ecommerce

Shopify for established or larger stores; standard themes for ordinary customisation, Hydrogen + Oxygen when a custom headless storefront is justified (it's a separate architecture, not a theme plugin). WooCommerce when an existing WordPress investment, required plugins or mandatory self-hosting justifies it. Don't custom-build core commerce.

## Website launch flow

1. **`main` before launch:** an on-brand, fast coming-soon page on the real domain with correct metadata (plus a notify-me form only if launch-list collection is part of the project).
2. **`test` during development:** the real build on `test.<domain>`, behind Cloudflare Access and `noindex` (noindex alone is not access control); test data, secrets and email isolated from production.
3. **Launch:** validate, then merge `test` into `main`; verify production config, migrations, forms and indexing. For GSD roadmaps, put infrastructure and the coming-soon page in Phase 1.

## Keep choices current

At setup, verify current stable releases and supported integrations from official docs, Context7 or registries - not memory. Follow vendor docs over old tutorials. Pin Docker images and keep lockfiles; enable Renovate or Dependabot. Flag deprecated or unmaintained choices and name a replacement. Record the actual components, deployment targets, schema/auth owners, backup/restore approach and exceptions in project docs; keep optional tools out of deployment until a requirement activates them.

## Personal overrides

Hosts, domains, shared-service URLs, tunnel IDs and accounts are not stored in this repo. Put them in the user's own CLAUDE.md or a private local skill.
