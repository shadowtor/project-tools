---
name: claude-md-cleanup
description: Audit a CLAUDE.md (or AGENTS.md) line by line and cut what doesn't earn its place - vague advice, things the model would do anyway, and duplicated lines - showing a keep/cut table for approval before touching the file. Use whenever the user says their CLAUDE.md is bloated, messy, too long, or "cleanup my claude.md", asks to prune project instructions or memory files, or wants to check that every instruction is actually changing behaviour.
---

# CLAUDE.md cleanup

Every line in CLAUDE.md is loaded into every session, so each one has to change what Claude does in this codebase. Lines that don't are noise that dilutes the ones that do.

## Process

1. Read the file (and any files it imports). If there are several (global `~/.claude/CLAUDE.md`, project, nested), ask which one, or default to the project file and treat the global one as read-only context for spotting duplicates.
2. For **every line** ask: *if I removed this, would I do something wrong in this codebase?* Check against the code where you can (does the command, path or convention it names still exist?).
3. Produce a table with three columns: **the line**, **keep or cut**, **why**. Group consecutive lines that stand or fall together.
4. Cut:
   - anything **vague** ("write clean code", "be careful"),
   - anything you'd **do anyway** (default behaviour, obvious language conventions),
   - anything that **repeats another line** (keep the clearest one),
   - anything **stale** (names files, commands, versions or tools that no longer exist).
5. Keep what is non-obvious and enforceable: project-specific commands, gotchas that caused real mistakes, hard constraints, pointers to where things live. Prefer rewriting a vague-but-important line into a specific one over cutting it, and say so in the "why".
6. **Show the table first and wait.** Don't edit the file until the user approves; then apply exactly the approved changes and show the resulting line count.

Tell the user, in one line at the end, the lines-before/lines-after count.
