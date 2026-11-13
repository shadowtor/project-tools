# Platform, infrastructure and logic holes (20)

Items marked **(USER ACTION)** are often DNS, hosting or provider settings; give the exact setting to check instead of guessing.

| # | Hole | How to check | Fix |
| --- | --- | --- | --- |
| 1 | Auth checked only in Next.js middleware | Next.js version; middleware-only gating of routes | Move the check into the route handler or server action (and upgrade to a patched version) |
| 2 | `.git` folder served in production **(USER ACTION)** | Request `/.git/HEAD` on the live site; look at Dockerfile/deploy copying the repo root | Deploy built output only; block `/.git`; rotate any key ever committed |
| 3 | Open storage/database rules | Firebase/Supabase storage and row rules; bucket visibility | Private buckets; policies scoped to owner; tighten before deploy |
| 4 | Price trusted from the browser | Checkout code: does the client send an amount? | Client sends a product id; server recomputes the amount |
| 5 | No credential-stuffing defence | Login route: per-user and per-IP limits, MFA support, breached-password check | Add per-user/per-IP rate limits, MFA, breached-password check |
| 6 | Database reachable from the internet **(USER ACTION)** | Compose/Docker port bindings, DB bind address, provider firewall; restore a backup | Private network only, auth required; verify a backup actually restores |
| 7 | Third-party scripts from a CDN | List external `<script src>`; abandoned vendors | Self-host or pin with Subresource Integrity; drop abandoned ones |
| 8 | Send endpoints with no limit | Any route that sends SMS or email | Rate limit; cap destination countries |
| 9 | AI feature with too much access | Tool permissions, secrets in prompts, irreversible actions | Least access; no secrets in instructions; confirm before anything irreversible |
| 10 | Weak or hardcoded JWT secret | JWT config; env vars | Long random secret from the environment; verify the signature on every request |
| 11 | Race conditions on limits | One-time codes, balances, quotas implemented as read-then-write | One atomic DB step (transaction, conditional update, unique constraint) |
| 12 | No email verification; unsafe social-login merge | Signup and OAuth account-linking code | Verify email first; only merge social accounts on verified email |
| 13 | DNS pointing at deleted services **(USER ACTION)** | List subdomain records and their targets | Delete records pointing to anything removed (subdomain takeover) |
| 14 | Server fetches user-given URLs (SSRF) | `fetch(userUrl)`, image proxies, webhooks, link previews | Don't fetch user URLs from inside the network; allowlist hosts; require IMDSv2 on AWS **(USER ACTION)** |
| 15 | User input reaching filesystem paths | `fs.*`, `path.join` with request data | Never pass user input to file APIs; patch server software |
| 16 | Reset links built from the Host header | Password-reset email link construction | Build links from configured base URL, not `req.headers.host` |
| 17 | Source maps shipped to production | Build config, deployed `.map` files; what a package contains before publish | Disable production source maps; inspect packed contents (`npm pack --dry-run`) |
| 18 | No framing protection | Response headers | `X-Frame-Options` or CSP `frame-ancestors` |
| 19 | Redirects from user input | `?next=`, `redirect(req.query...)` | Allowlist or relative-path-only validation |
| 20 | Login/signup/reset leaks which emails exist | Differing messages or timings between known and unknown emails | Same generic response either way |
