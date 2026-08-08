---
description: Pull a shared skill from the Brian-Hanson helpers repo into this project
argument-hint: [skill-name]
allowed-tools: WebFetch, Write
---

Fetch the skill named `$ARGUMENTS` (if no name is given, default to
`architect-blueprint`) from the public repo at
https://github.com/bjhorg-prog/Brian-Hanson and install it into this project.

Known skills and the files each one needs (raw GitHub URL to fetch, and the
local path to write it to):

**architect-blueprint**
- https://raw.githubusercontent.com/bjhorg-prog/Brian-Hanson/main/.claude/skills/architect-blueprint/SKILL.md → `.claude/skills/architect-blueprint/SKILL.md`
- https://raw.githubusercontent.com/bjhorg-prog/Brian-Hanson/main/references/blueprint-method.md → `references/blueprint-method.md`

Steps:
1. Look up the requested skill name in the table above. If it isn't listed,
   say so and stop — don't guess a URL for it.
2. Fetch each file for that skill with WebFetch.
3. Write each file's content to its matching local path exactly as fetched,
   creating any missing directories.
4. Report which files were written and where, and tell the user the skill is
   ready to use.

If a fetch fails (404, network error), report the exact failure and do not
write a partial or placeholder file.
