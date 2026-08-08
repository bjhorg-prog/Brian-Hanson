# Brian Hanson — Helpers

Reusable Claude Code skills, shared across client projects.

## One-time setup (per project)

Paste this into a fresh Claude Code chat in your project:

> Fetch https://raw.githubusercontent.com/bjhorg-prog/Brian-Hanson/main/.claude/commands/get-skill.md and save it to .claude/commands/get-skill.md in this project, creating the .claude/commands/ folder if it doesn't exist.

## Using it

Once installed, run:

```
/get-skill architect-blueprint
```

to pull that skill (and everything it depends on) into your project. Run it
again any time to refresh to the latest version.

## Skills available

- **architect-blueprint** — turns an idea into a build-ready blueprint
  (action/reaction/counteraction, premortem, risk table, unknowns ledger,
  hard stops) that a cheaper model or a later session can execute without
  improvising.
