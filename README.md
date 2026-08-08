# Brian Hanson — Helpers

Reusable Claude Code skills, shared across client projects.

## One-time setup (per project)

Run this in your project directory — a real shell command, not a "fetch this
URL" request to Claude. (Asking Claude to "fetch" it can route through
WebFetch, which processes content through an AI model and will corrupt the
file instead of copying it exactly.)

```
mkdir -p .claude/commands && curl -fsSL https://raw.githubusercontent.com/bjhorg-prog/Brian-Hanson/main/.claude/commands/get-skill.md -o .claude/commands/get-skill.md
```

Paste that into your terminal directly, or paste it into Claude Code and ask
it to run it (it will use its Bash tool, not WebFetch).

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
