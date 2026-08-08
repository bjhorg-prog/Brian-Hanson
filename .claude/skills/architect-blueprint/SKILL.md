---
name: architect-blueprint
description: Turn one of the user's ideas into a build-ready blueprint using the Blueprint Method (action/reaction/counteraction, premortem, risk table, unknowns ledger, hard stops). Use when the user says "blueprint this", "architect this", "run the blueprint method on X", or brings an idea they want planned so a cheaper model or later session can execute it without improvising. Architect only — this skill never builds anything.
---

# Architect Blueprint

One job: turn an idea into a blueprint a foreman can execute blind.
The method lives in `references/blueprint-method.md` — read it in full before
starting. This skill is the workflow; that file is the substance.

This is an **architect** pass: read, probe, verify, design. Build nothing, fix
nothing, change no state (read-only recon commands are allowed). Execution is a
separate session/model working from the finished blueprint.

Model note: architect passes deserve the strongest available thinking model.
Recommend switching to Opus 5 if it isn't already active before starting. If
the user is running on a lighter model even after that recommendation, say so
plainly and let them decide.

## Step 1 — Brief (checkpoint)

Take the user's idea and write the brief backwards from the outcome: what does
done look like, why does it matter, what is explicitly out of scope. Present it
in a few sentences and get their confirmation before designing anything. If
"done" is genuinely ambiguous, ask one question — not five.

## Step 2 — Recon

Verify the environment the build will land in. Open the real files, run the
read-only checks, read the relevant `session-log/` entries for prior work on
the same ground. Every fact in the blueprint gets tagged [verified] or
[assumed]; the point of recon is to shrink the [assumed] list before design
starts. Check `files/blueprints/UNKNOWNS.md` — open unknowns from past builds
may already touch this one.

## Step 3 — Draft the blueprint

Write `files/blueprints/BLUEPRINT-<slug>.md` with every section from
`references/blueprint-method.md` in order: brief, context, goals/non-goals,
decisions with door tags, the F-numbered build map (action / reaction /
counteraction / trigger / evidence), premortem, risk table, U-numbered
unknowns ledger, cheapest-test-first phasing, rollback, hard stops, handoff
block. Right-size it — mini-blueprint for small ideas, full for big builds.

## Step 4 — Unknowns checkpoint

Present the user the U-list only: each unknown, what it blocks, and a
recommended answer where one exists. They answer what they can. Fold answers
back in and re-check the affected sections. Repeat until nothing changes.
Unanswered unknowns stay flagged and gate their phases — they do not get
guessed into answers.

## Step 5 — Gate and hand off

1. Score the blueprint 1-10 against `references/blueprint-method.md`
   (architect's rules section). Below 9 is not finished — fix it, don't
   present it.
2. Copy still-open unknowns to `files/blueprints/UNKNOWNS.md` (create it on
   first use; append-and-resolve, never delete).
3. Add a `session-log/` entry: blueprint path, headline risks, open U-numbers,
   and which phase the foreman should run first.
4. Tell the user how to execute it: hand the blueprint file to a cheaper model
   or a fresh session with the instruction to follow its handoff block and
   foreman's rules.

## Hard rules

- Never execute the blueprint in the same session that wrote it unless the
  user explicitly says "now build it" — and then only after the gate.
- Unknowns are flagged, never silently guessed. Hard stops are copied into
  every blueprint, including the workspace standing stops.
- No emoji. Plain technical prose — blueprints are working documents, not
  content.
- One checkpoint question at a time.
