# Scanner-found code issues (10 items)

Source: https://www.murphmaxxing.com/vault/10-more-ways-to-get-your-app-sued - verify current law and penalties there.

These are found by automated scanners and demand-letter firms, so the app doesn't need to be noticed first.

| # | Risk | How to check in code | Fails when | Fix |
| --- | --- | --- | --- | --- |
| 1 | Marketing SMS without consent (TCPA) | SMS provider calls (Twilio, Vonage, etc.); consent fields; STOP/HELP handling; send-time logic | Marketing texts sent with no stored opt-in (timestamp + IP), no STOP/HELP handling, or no 8am-9pm local quiet hours | Separate transactional from marketing SMS; unchecked consent box; store proof; handle STOP/HELP |
| 2 | User uploads, no DMCA route | Upload features (images, posts, audio); `/dmca` page; takedown route | UGC exists and there's no published designated-agent contact or takedown route | Register an agent with the Copyright Office, publish contact on `/dmca`, add a takedown route |
| 3 | PII unencrypted at rest | Schema for personal data columns, DB/storage encryption config, RLS, admin MFA, breach-notification runbook | Plain-text sensitive fields, no encryption at rest, RLS off, no written notification step | RLS on, encryption at rest, MFA on admin, written 72-hour notification step |
| 4 | Chatbot may state prices/refunds/policies | System prompts and chat route | The prompt doesn't forbid stating prices, refunds, medical, legal or financial claims; policies generated rather than read from a source of truth; no logging; no "confirm with support" line | Forbid in prompt, pull policy text from a source of truth, log conversations, add visible disclaimer |
| 5 | Face/voice/avatar features without consent | Face login, voice cloning, selfie-derived avatars | Biometric capture with no written consent screen, retention schedule, or deletion path | Consent screen before capture naming what/why/how long; published retention schedule; never sell templates |
| 6 | Pixel/analytics before consent | Layout/head files, tag managers, Meta/GA4/TikTok/other pixels, consent component | Any non-essential tag fires on page load before the user opts in (check the Network tab in a private window), or tags on video/sensitive pages | Consent tool that actually blocks tags until opt-in; no pixels on video or sensitive flows |
| 7 | Accessibility basics | Missing `alt`, no visible focus states, low contrast, modals not keyboard-closable, `div` buttons | Images without alt, unreachable controls by keyboard, contrast below 4.5:1 | Run axe/Lighthouse, fix red items, tab through every flow; don't buy an overlay |
| 8 | No age gate at signup | Signup form and flow | Account creation with no neutral age question on an app minors will use | Neutral age gate; block under 13 unless full parental-consent flow; use platform age-range APIs on mobile; no behavioural ads to under-18s |
| 9 | Marketing email without unsubscribe | Email templates, sending loops, list sources | No one-click unsubscribe, no postal address, misleading from/subject, purchased lists | One-click unsubscribe + postal address in every marketing email; honour opt-outs within 10 business days |
| 10 | "AI-powered" claims without proof | Landing/marketing copy, README, store listings | Capability claims ("detects", "diagnoses", "guarantees", accuracy %) with no reproducible test result behind them | Back each claim with a test result; say "assists" not "detects" unless measured |
