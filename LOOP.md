# PRODUCTION LOOP

One cycle produces and publishes one chapter, unattended. Any orchestrator agent (fresh context,
zero chat history) runs a cycle from this file alone. **CONTINUITY IS A FILE, NEVER A CONTEXT
WINDOW** — cold-start truth is exactly: `bible/` + `continuity/log.md` + `chapters.json` + `engine/`.

Roles, fixed: **Opus 5 writes** (bible, breakdowns, dialogue, continuity, site). **GPT-5.6 Sol +
ImageGen draws** (pages, covers). The writer never draws. The artist never invents story — it
renders a finished page block.

## The cycle

0. **Sync + orient.** `git pull`. Read `chapters.json` → next chapter N. Read `continuity/log.md`.
   If the current season's 12 chapters are all published: FIRST spawn the writer to append a
   SEASON 2 (or next) 12-chapter outline to `bible/arc.md`, commit, then continue with its ch 1.
   The series never ends.
1. **Write.** Spawn an Opus 5 writer agent with: `bible/series.md`, `bible/cast.md`, `bible/style.md`,
   the chapter's row in `bible/arc.md`, the FULL `continuity/log.md`, and `engine/chapter-engine.md`.
   It writes `production/chNN/breakdown.md` per the engine grammar. It sees no other chat context.
2. **Lint.** `pwsh scripts/lint-breakdown.ps1 -Chapter N`. On violations, send the exact violation
   list back to the writer for a targeted fix. Drawing never starts on a failing breakdown.
3. **Build prompts.** `pwsh scripts/build-prompts.ps1 -Chapter N` → `production/chNN/prompts/pNN.txt`
   (preamble + full ART SPEC + character locks + page script; see engine/page-prompt-template.md).
4. **Calibration gate.** `pwsh scripts/draw-pages.ps1 -Chapter N -Pages "01,<densest>"` where
   <densest> = the mid-chapter page with the most bubbles (lint prints it). OPEN both PNGs and
   COUNT panels, bubbles, faces per engine/chapter-engine.md. Zero bubbles or zero faces is a
   failure to fix, not a variation to report. Only a passing gate unlocks step 5.
5. **Draw.** `pwsh scripts/draw-pages.ps1 -Chapter N` — draws every missing page, 3 in parallel,
   full ART SPEC on every single call, retry once per page, verifies every PNG on disk. Rerun the
   same command to fill any stragglers; it is idempotent.
6. **Cover.** `pwsh scripts/build-prompts.ps1 -Chapter N -Cover` then
   `pwsh scripts/draw-pages.ps1 -Chapter N -Cover` (needs the pages done first — the cover is
   generated after its chapter exists).
7. **Publish.** `pwsh scripts/publish.ps1 -Chapter N -Title "<chapter title>"` →
   downscales raw PNGs to `chapters/chNN/pNN.jpg` (~900px wide, JPEG q75, ~200 KB) + `cover.jpg`,
   appends the chapter entry to `chapters.json`, appends CONTINUITY NOTES to `continuity/log.md`
   (date-stamped), commits **web-sized images + text only** (raw PNGs are gitignored), pushes.
8. **Verify + repeat.** Confirm the push and the live URL. Start the next cycle at step 0.

## Invariants (do not renegotiate these mid-loop)

- Full ART SPEC on every image call. Never abbreviated, never "as before".
- No chapter under 25 pages; no page under 6 panels (splash excepted); no page under 8 bubbles
  (splash 3-6); no silent pages anywhere.
- Exactly one splash per chapter. At least 3 chibi panels. A domestic beat every chapter.
- The gate runs for EVERY chapter, not just the first.
- Full-res raws never enter git history. Web-sized JPEGs only.
- The log is append-only. Never rewrite history in it.

## Unattended operation

The orchestrator session runs cycles back-to-back via scheduled wakeups (/loop dynamic mode),
one chapter per cycle. If the session dies, restart is one message in Claude Code at the repo:
**"Run one production cycle per LOOP.md."** Nothing else is needed — the repo is the memory.
