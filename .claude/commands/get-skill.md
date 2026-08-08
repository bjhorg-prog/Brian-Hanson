---
description: Pull a shared skill from the Brian-Hanson helpers repo into this project
argument-hint: [skill-name]
allowed-tools: Bash
---

Install the skill named `$ARGUMENTS` (if no name is given, default to
`architect-blueprint`) from the public repo at
https://github.com/bjhorg-prog/Brian-Hanson into this project.

Do NOT use WebFetch for this. WebFetch runs fetched content through an AI
model rather than returning it verbatim, and will corrupt these files —
confirmed by testing: fetching a skill file through WebFetch returned a
paraphrased summary instead of the real content, which breaks a SKILL.md's
YAML frontmatter. Use a raw byte fetch instead: `curl` via Bash, or
`Invoke-WebRequest -OutFile` via PowerShell if curl isn't available.

Known skills and the files each one needs (raw GitHub URL to fetch, and the
local path to write it to):

**architect-blueprint**
- https://raw.githubusercontent.com/bjhorg-prog/Brian-Hanson/main/.claude/skills/architect-blueprint/SKILL.md → `.claude/skills/architect-blueprint/SKILL.md`
- https://raw.githubusercontent.com/bjhorg-prog/Brian-Hanson/main/references/blueprint-method.md → `references/blueprint-method.md`

Steps:
1. Look up the requested skill name in the table above. If it isn't listed,
   say so and stop — don't guess a URL for it.
2. Create any missing local directories.
3. For each file, run `curl -fsSL <raw-url> -o <local-path>` (a raw byte
   fetch — not WebFetch).
4. Verify each written file is non-empty and starts with the expected content
   (e.g. `---` frontmatter for SKILL.md) before reporting success.
5. Report which files were written and where, and tell the user the skill is
   ready to use.

If a fetch fails (curl exit code, 404, network error), report the exact
failure and do not leave a partial or placeholder file in place.
