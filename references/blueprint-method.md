# The Blueprint Method — architect-grade planning

Single source of truth for how blueprints get written in this workspace. The
`architect-blueprint` skill runs this method; this file holds the substance.

Origin: "The Blueprint Method" field guide (Usman Mohammed, C2D Inc., 2026) —
action / reaction / counteraction on every fork, flagged assumptions, hard stop,
architect/foreman split. This file extends that core with practices from
veteran-engineer planning: Google-style design docs (goals/non-goals,
alternatives considered, trade-offs), Gary Klein's premortem, FMEA-style risk
ranking, Amazon's one-way/two-way door test, and cheapest-test-first phasing.

## The operating model

- **Architect** (strongest model, one pass): produces the blueprint. Writes no
  build code. Its entire output is the map.
- **Foreman** (cheaper model or later session): executes the blueprint. Never
  improvises — every fork it hits was already decided. Records what actually
  happened in `session-log/`.
- **The user** stays in the loop at the checkpoints only: confirming the
  brief, answering unknowns, approving the handoff. Everything a blueprint can
  decide in advance, it decides in advance.

## The core loop

1. Brief — outcome, not steps.
2. Recon — verify the real environment before designing for it.
3. Blueprint — the full document below.
4. Iterate — the user answers the flagged unknowns; re-run until nothing
   changes.
5. Hand off — to the foreman, with a session-log trail.

## Blueprint document anatomy

Every blueprint is one markdown file: `files/blueprints/BLUEPRINT-<slug>.md`.
Sections, in order. Skip a section only by writing "N/A — <reason>", never
silently.

### 1. Brief
Outcome, what "done" looks like, why it matters. Written backwards from the
result, not forwards from the first step. One paragraph. If "done" can't be
stated crisply, the blueprint stops here and asks.

### 2. Context and scope
Objective facts about the current environment. Every fact is tagged
**[verified]** (the architect actually looked — file read, command run, doc
checked) or **[assumed]** (goes to the unknowns ledger, §8). Most builds die on
an unverified fact — which account owns which credential, which two systems
actually talk, where the data really lives. Recon exists to shrink this list.

### 3. Goals and non-goals
Non-goals are the load-bearing half: things that could reasonably be goals but
are deliberately out. A blueprint without non-goals grows until it fails.

### 4. Decisions, trade-offs, alternatives
For each significant design decision:
- The choice made, and the trade-off accepted (what got worse on purpose).
- At least one real alternative considered, and why it lost. "No alternative
  existed" must be stated, not implied.
- **Door tag:** `[two-way]` — reversible, decide fast, don't over-plan;
  `[one-way]` — hard to undo, gets a rollback plan (§10) and, where possible,
  gets converted into a two-way door first (smaller scope, a copy, a test
  target, a dry run).

### 5. Build map — action / reaction / counteraction
The heart of the method. Numbered steps (F1, F2, ... — findings/fixes keep
their numbers forever, matching prior blueprints). For every step:

- **Action** — what the foreman does.
- **Reaction** — the most likely way reality pushes back.
- **Counteraction** — decided now, not improvised later.
- **Trigger** — observable condition per fork: "if you see X, take path A; if
  you see Y, take path B." A trigger must be something the foreman can actually
  observe (an error message, a file's presence, an exit code), not a judgment
  call.

Every step also names its **evidence of completion** — the observable proof it
worked (test output, file exists, screenshot, log line). "It should work now"
is not evidence.

### 6. Premortem
Before finalizing, the architect writes: *"It is six months later. This build
shipped and failed anyway. What happened?"* — then lists the most plausible
causes, independent of the build map (imagining the failure as already real
surfaces ~30% more risks than asking "what might go wrong"). Each cause is then
either: mapped to a counteraction in §5, added to the risks table in §7 as an
accepted risk, or promoted to an unknown in §8. Premortem causes that the build
map already covers are listed anyway, with the F-number that covers them —
that's the audit.

### 7. Risk table (FMEA-lite)
For the risks that survive the premortem, a small table:

| Risk | Likelihood (1-3) | Impact (1-3) | Would we even notice? (1-3, 3 = silent) | Score | Disposition |
|------|------------------|--------------|------------------------------------------|-------|-------------|

Score = product. Anything ≥ 12 must get a counteraction or an explicit
"accepted, because..." from the user. The third column is the FMEA insight
most plans miss: a failure nobody notices is worse than a loud one — silent
risks get detection added (a check, an alert, a verification step), not just
a fix.

### 8. Unknowns ledger
Numbered U1, U2, ... Every assumption the architect cannot resolve alone.
Rules:
- An unknown is **flagged, never guessed at silently**. Guessing is allowed
  only when labeled: "Proceeding as if X — flagged, reversible, here's the
  undo."
- Each unknown states what it blocks, so the user can prioritize answers.
- Unknowns that outlive the blueprint get copied to the master ledger,
  `files/blueprints/UNKNOWNS.md`, so nothing is silently forgotten between
  builds. Answered ones are marked answered there, with the answer — the
  ledger is append-and-resolve, not delete.

### 9. Phasing — cheapest test first
Order the build so the cheapest, most informative step runs first:
- If a core assumption is untested, phase 1 is a **spike** — the smallest
  possible probe that proves or kills it.
- If the build has many parts, phase 1 is a **walking skeleton** — the
  thinnest end-to-end version that touches every part once, before any part is
  built deep.
- Each phase ends with its evidence of completion and a go/no-go.
Expensive certainty last, cheap information first.

### 10. Rollback
Every `[one-way]` decision and every step that modifies existing state gets an
undo written down: what to restore, from where, verified how. "Take a backup"
is only a rollback plan if the blueprint says where the backup goes and how
restore is tested.

### 11. Hard stops
The exact conditions where the foreman quits and waits for a human instead of
pushing forward. Always includes this workspace's standing stops:
- Never read or print secrets (`.env`, credential/oauth JSONs, keys).
- Nothing is sent or published externally — drafts only.
- Stop on any privacy-sweep hit (other people's private data).
Plus the build-specific stops the architect defines.

### 12. Handoff block
The last section, written for the foreman: which phase to run first, which
file paths are in play, which U-answers gate which phases, and the instruction
to log execution results in `session-log/` — including anything **deferred by
design** (a decision) vs. skipped (a failure), which are never the same thing.

## Architect's rules

- One pass, no bricks: the architect reads, probes, and verifies, but does not
  build, fix, or "quickly clean up" anything. (Read-only recon commands are
  fine; state changes are not.)
- Evidence over vibes: prefer one verified fact to three plausible guesses.
  During recon, actually open the files and run the read-only checks.
- The blueprint must survive its author's absence: a competent stranger — or a
  cheaper model — should be able to execute it without asking anything that
  isn't already a numbered unknown.
- Right-size it: a small idea gets a 1-2 page mini-blueprint (all sections,
  shorter); a big build gets the full treatment. If the whole thing is one
  obvious reversible step, say so and don't blueprint it — overhead is also a
  failure mode.
- Rate before it ships: score the blueprint 1-10 against this file (all
  sections present or explicitly N/A, triggers observable, unknowns flagged
  not guessed, hard stops concrete). Below 9 is not finished.

## Foreman's rules

- The blueprint is the authority; the foreman's judgment is for observations
  ("which trigger fired"), not decisions ("which path is better").
- Respect every hard stop, literally.
- Verify, then claim: report evidence of completion, not intentions.
- Reality wins: when the environment contradicts the blueprint (a file that
  should exist doesn't), that's a trigger mismatch — stop the affected step,
  log it, continue unaffected steps. Never patch the plan mid-flight.
- Close the loop: session-log entry with what shipped, what was deferred by
  design, what mismatched, and any new unknowns for the ledger.
