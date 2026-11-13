---
name: ux-details-builder
description: Check an app against 20 high-end UX details (skeleton loaders, optimistic updates, Cmd+K command menu, shortcut hints, undo toasts, stale-while-revalidate, prerendering, BlurHash placeholders, view transitions, swipe-to-dismiss sheets, interruptible springs, predictive back, hit slop, safe-area insets, one-time-code autofill, tabular numbers, balanced headlines, continuous corners, idempotency keys, safe-triangle menus) and build the ones that fit, one at a time, with reduced-motion respected. Use whenever the user wants UX polish, "make it feel native/premium", micro-interactions, a UX review that ends in real changes, or pastes a numbered list of UX details to implement - even if they don't name specific ones.
---

# UX details builder

Walks the 20 details in `references/ux-details.md` against the project, builds the ones that fit, and reports every item as Built, Already done, or Skipped.

## Rules

- **Reuse before adding.** Use what the project already has (component library, animation lib, data-fetching lib) before adding a dependency, and match the existing code style. Say so if a new dependency is truly needed and ask first.
- **Reduced motion.** Every animation honours `prefers-reduced-motion` (CSS media query, or the animation library's reduced-motion option).
- **One at a time.** Build each detail separately and run the project's checks (typecheck, lint, tests, build) after each before starting the next. A failing check stops you until it's fixed.
- **Skip honestly.** Skip a detail when it doesn't fit the project type (e.g. Android predictive back in a web-only app), when it needs backend work you can't do from here, or when its own "don't use for" clause applies (payments, balances and stock counts for optimistic/cached data; marketing sites for a command menu). Say which clause.
- Don't add unrequested UI. A detail is built only where the app has a place for it.

## Process

1. Detect platform and stack (web framework and version, React Native/Expo, SwiftUI, Android), the component and animation libraries in use, and what is already in place (search before building).
2. Produce a table of all 20: `# | detail | status (Build / Already done / Skip) | where or why`. Show it, then proceed with the Build items unless the user redirects.
3. Build in the order that minimises rework: foundations first (safe-area, tabular numbers, balanced headlines, hit slop, idempotency keys), then data behaviour (skeletons, optimistic updates, stale-while-revalidate, undo toasts), then navigation and motion (command menu, shortcut hints, prerendering, view transitions, sheets, springs), then polish (BlurHash, continuous corners, safe triangle).
4. After each, note how the user can see it working (URL/route, interaction, device setting).

## Final report

List all 20, each marked:
- **Built** - what changed, which files, how to see it working.
- **Already done** - where it lives.
- **Skipped** - why.

Browser-verify web changes yourself (drive the page, check the interaction and reduced-motion behaviour) before asking the user to check anything.
