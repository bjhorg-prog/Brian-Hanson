# Brian Hanson — Helpers

Reusable Claude Code skills, shared across client projects.

## Getting a skill into your project

1. Clone this repo as a sibling folder, next to your project (not inside
   it) — the same way course material or any other reference repo gets
   cloned alongside a working project:

   ```
   git clone https://github.com/bjhorg-prog/Brian-Hanson.git
   ```

2. Copy the skill you want from the clone into your project. For
   `architect-blueprint`, that's two files:

   ```
   cp -r ../Brian-Hanson/.claude/skills/architect-blueprint .claude/skills/
   cp ../Brian-Hanson/references/blueprint-method.md references/
   ```

3. That's it — no install script, nothing auto-executed. You can open and
   read both files before copying them if you want to see exactly what
   they do.

To update later, `git pull` inside the cloned `Brian-Hanson` folder, then
re-copy the files you're using.

## Why not an automated install command

An earlier version of this repo shipped a `/get-skill` command that fetched
files over HTTP and immediately executed their contents. In testing, that
pattern — fetch unreviewed remote content, then blindly act on it — got
flagged and blocked by Claude Code's own safety systems, the same way a
supply-chain attack would be. Plain `git clone` avoids that entirely: it's
an ordinary, transparent operation, and nothing runs until you decide to
copy a file into a place where it takes effect.

## Skills available

- **architect-blueprint** — turns an idea into a build-ready blueprint
  (action/reaction/counteraction, premortem, risk table, unknowns ledger,
  hard stops) that a cheaper model or a later session can execute without
  improvising.

## Task management with Claude Code + Todoist

Claude Code can manage your task list conversationally through Todoist — add, complete,
reschedule, and check on tasks just by asking, no manual setup beyond auth.

1. If you don't already have a Todoist account, sign up free at https://todoist.com — the
   setup below will prompt you to sign in/sign up automatically if you skip this step.

2. Install the Todoist CLI and skill:

   ```
   npm install -g @doist/todoist-cli
   td skill install claude-code
   td auth login
   ```

   `td auth login` opens a browser window for Todoist's OAuth sign-in (no manual token or
   config file needed).

3. In Claude Code, run `/mcp` to confirm the Todoist connection is active. From there, just
   ask Claude to add, list, or complete tasks in plain language.

Full setup docs: https://www.todoist.com/help/articles/use-claude-code-with-todoist-cli-and-mcp-b1USJ4HB3
