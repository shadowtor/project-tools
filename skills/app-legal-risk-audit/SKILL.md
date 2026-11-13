---
name: app-legal-risk-audit
description: Audit an app's codebase for the legal and compliance mistakes that get small apps sued, fined or taken down - privacy policy gaps, tracking before consent, public storage buckets, fake testimonials, hard-to-cancel subscriptions, SMS and email consent, DMCA and takedown routes, age gates, accessibility, AI disclosure and chatbot claims, cookie banners, urgency timers, algorithmic pricing labels, and EU/UK/US app laws. Use this whenever the user asks for a compliance check, legal audit, "is my app breaking the law", "could I get sued", a pre-launch legal review, a cookie or privacy review, or pastes one of the "ways to get your app sued" checklists - even if they don't say "legal".
---

# App legal risk audit

Walks a codebase against three checklists of the legal risks that most often hit fast-built apps, and reports each as PASS, FAIL or CAN'T TELL with evidence. This is an engineering checklist, not legal advice - say so once in the report and recommend a licensed professional for anything that takes money or personal data.

## The three lists

Read only the lists that fit the app (default: all three, skipping items that clearly don't apply, such as mobile builds for a pure web app - say which you skipped).

| List | File | What it covers |
| --- | --- | --- |
| Paperwork and promises | `references/paperwork-and-promises.md` | Privacy policy accuracy, third-party disclosure, broken deletion promises, public storage, fake reviews, cancellation, trials, companion-chatbot safety (10 items) |
| Scanner-found code issues | `references/code-level-risks.md` | SMS consent, DMCA, unencrypted PII, chatbot guardrails, biometrics, pixels before consent, accessibility, age gate, unsubscribe, AI claims (10 items) |
| Regulator and portal laws | `references/regulator-laws.md` | Image licences, cookie banners, report buttons, health data in analytics, urgency UI, AI-therapy claims, pricing disclosure, takedown SLA, AI labels, security contact (11 items) |

If the user names a starting item ("start with number 6") or a single list, honour it. Items marked **silent** in the reference files (they get worse without any visible symptom, e.g. a public storage bucket) go first when no order is given.

## How to run it

1. **Orient.** Identify the stack, whether the app has accounts, payments, uploads, user-generated content, AI features, analytics/pixels, email/SMS, mobile builds, and which regions it serves. Skip nothing silently.
2. **Check each item** using the "How to check" line in the reference file. Search the code and config for the specific patterns; read the files rather than trusting a grep hit. Verdicts:
   - **PASS** - cite the file that satisfies it.
   - **FAIL** - cite `file:line` of the problem (or the place where the missing thing should exist).
   - **CAN'T TELL** - say exactly what to look at (a cloud console setting, the live privacy policy, a DNS record) because the code can't answer it.
3. **Rank the failures** by how likely each is to hurt the app and how fast it gets worse, then give a fix order. Public data exposure and broken deletion promises usually lead; document fixes come after.
4. **Fixes.** Default to report-only. If the user asked for diffs ("write me the diffs"), produce a diff per failure but do not apply anything until they approve; apply approved fixes one at a time and run the project's checks after each.

## Report format

Lead with a one-line score (`N fail / N can't tell / N pass`), then a table: `# | item | verdict | file:line | fix`. After the table, the ranked fix order, then diffs if requested. No narrative summary of the checklist itself - the user already has it.

## Guardrails

- Never invent evidence. If you can't see it, it's CAN'T TELL.
- Don't write legal text (privacy policy, terms) as if final; draft from what the code actually does and flag it for review. A policy that contradicts the code is itself the violation, so generate it from the code, not a template.
- Don't edit production config, rotate keys, or publish anything without asking.
- Dollar figures and law dates change; if you quote them, tell the user to verify against the source linked in the reference file.

Source lists adapted from the Murphmaxxing vault articles linked at the top of each reference file.
