# Project Tools

Agent skills for starting, auditing and polishing software projects. Each skill is a plain `SKILL.md` folder, so they work with Claude Code and any agent that supports the Agent Skills format.

| Skill | What it does |
| --- | --- |
| [`project-starter`](skills/project-starter) | Picks a stack from opinionated defaults, sets up baseline dev tooling (Gitleaks, Trivy, Semgrep, ZAP, ...), the Claude toolbelt and a CLAUDE.md template, and creates a baseline-audit phase |
| [`app-legal-risk-audit`](skills/app-legal-risk-audit) | Audits a codebase against 31 legal/compliance risks (privacy, consent, takedown, accessibility, AI disclosure, regulator laws) with `file:line` evidence |
| [`security-holes-audit`](skills/security-holes-audit) | Report-first audit against 40 common security holes; fixes only what you approve, one at a time |
| [`ux-details-builder`](skills/ux-details-builder) | Checks 20 premium UX details and builds the ones that fit, respecting reduced motion |
| [`claude-md-cleanup`](skills/claude-md-cleanup) | Keep/cut table for every line of a CLAUDE.md, applied only after approval |

## Install

**Claude Code plugin** (recommended):

```
/plugin marketplace add shadowtor/project-tools
/plugin install project-tools@project-tools
```

**Any agent / manual:** copy the folders you want from `skills/` into your agent's skills directory (for Claude Code: `~/.claude/skills/` or a project's `.claude/skills/`), or with the `skills` CLI:

```
npx skills add shadowtor/project-tools
```

## Use

Ask in plain language, or name the skill:

- "Set up this repo with our starter stack and baseline tooling" -> `project-starter`
- "Is my app breaking the law? Start with number 6" -> `app-legal-risk-audit`
- "Audit my codebase for security holes, report first" -> `security-holes-audit`
- "Make this feel more premium" -> `ux-details-builder`
- "My CLAUDE.md is bloated" -> `claude-md-cleanup`

## Notes

- The legal and security checklists are engineering aids, not legal or professional security advice. Laws, dates and penalties change: verify against the source linked in each reference file.
- Checklist content is adapted from the [Murphmaxxing vault](https://www.murphmaxxing.com/vault) articles linked in the references.
- `project-starter` encodes the author's defaults. Edit the files in `skills/project-starter/references/` to match your own preferences; personal hosts, accounts and secrets are deliberately not stored here.

## Contributing

Issues and pull requests welcome. Keep `SKILL.md` under ~500 lines, put long lists in `references/`, and keep descriptions specific about when the skill should trigger.

## Licence

MIT - see [LICENSE](LICENSE).
