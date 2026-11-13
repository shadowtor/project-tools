# Application-level holes (20)

Source: https://www.murphmaxxing.com/vault/20-security-holes-vibe-coded-app

| # | Hole | How to check | Fix |
| --- | --- | --- | --- |
| 1 | `.env` committed to git | `git ls-files | grep -i env`, `.gitignore`, history search for key patterns | Ignore `.env`, keep secrets server-side, rotate every key that ever touched a commit (history keeps them) |
| 2 | Real API keys in frontend code | `NEXT_PUBLIC_*`, `EXPO_PUBLIC_*`, `VITE_*` values; secrets in the built bundle | Only publishable keys client-side; real keys in server/edge functions |
| 3 | Row-level security off | Supabase RLS flags/policies, Firebase rules | RLS on by default; policies scoped to `auth.uid()`; test with a second account |
| 4 | Permissions checked in the frontend only | `if (user.isAdmin)` style checks without a matching server check | Every permission check runs on the server |
| 5 | No rate limiting | Login, signup, reset, AI and other expensive routes | Per-user and per-IP throttles |
| 6 | SQL built by string concatenation | Template strings or `+` building queries | Parameterised queries or an ORM |
| 7 | No server-side input validation | Handlers that use `req.body` directly | Validate and sanitise on the server with a schema library (e.g. Zod) |
| 8 | User content rendered as raw HTML | `dangerouslySetInnerHTML`, `innerHTML`, `v-html` | Render as text; sanitise with DOMPurify if HTML is required |
| 9 | Plaintext passwords | Password storage and comparison code | Auth provider hashing, bcrypt or argon2; never custom |
| 10 | Auth tokens in localStorage | `localStorage.setItem` with tokens | httpOnly cookies on web; secure storage on native |
| 11 | Admin/debug routes with no auth | `/admin`, `/debug`, env-dumping routes | Auth-gate every internal route; strip debug endpoints from production |
| 12 | CORS `*` | `Access-Control-Allow-Origin`, framework CORS config | Allowlist own origins only |
| 13 | No email verification | Signup flow, session creation | Verify email before access to anything real |
| 14 | Predictable IDs, no ownership check | Fetch/mutate handlers by id | Confirm the resource belongs to the requester on every fetch and mutation |
| 15 | Whole request body saved on update | `update(req.body)`, spread of input into models | Allowlist the fields a user may set (blocks `role: "admin"`) |
| 16 | Webhooks without signature check | Stripe and other webhook handlers | Verify signature against the webhook secret before touching the payload |
| 17 | Stack traces in production | Error handlers, framework debug flags | Generic user-facing errors; details to logs |
| 18 | Dependencies never updated | `npm audit` / equivalent, Dependabot/Renovate config | Audit, enable automated updates, patch on a schedule |
| 19 | No password strength/breach check | Min-length rules, provider leaked-password protection setting | Minimum length plus breached-password check (many auth providers ship one) |
| 20 | File uploads without validation | Upload handlers | Check type and size; store outside web root or in object storage; never execute uploads |
