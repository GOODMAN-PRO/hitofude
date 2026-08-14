# CHAPTER ENGINE

The rules that let ANY chapter be written and drawn with no further creative input.
A writer agent needs exactly four inputs: `bible/*`, `continuity/log.md`, `chapters.json`, and this file.
Its output is one file: `production/chNN/breakdown.md`.

## Chapter shape

- **25-32 pages, default 26-28.** Never compress to hit a number. If the outline runs short, add scenes — another domestic beat, a B-plot touch, a supporting-cast aside — never stretch single moments thin.
- Page 1 carries a caption box with `CHAPTER N — TITLE` **and** dialogue. The chapter opens mid-life, not on an establishing pin-up.
- **Exactly one SPLASH page** per chapter, placed at an emotional or comedic peak. It is a single full-bleed panel and it still carries 3-6 bubbles. There are no other full-page panels.
- **Panels: default 7 per page. 8 often. 6 sparingly** — six is a floor, not a target. (Splash: 1.)
- **Bubbles: default 10-12 per page.** Hard floor 8, hard ceiling 14. (Splash: 3-6.) Count every speech bubble and thought bubble; captions/asides/SFX don't count toward it.
- **Bubble length: 4-8 words, hard cap.** Density comes from the NUMBER of bubbles, never the length of one. Six quick lines across four small panels is the target texture. Split a long speech into a bubble chain.
- **Muttered asides: 2-4 per page.** Small unbubbled text beside a head. Use them often — they are the series' comedy engine.
- Thought bubbles carry the private-face interiority; both leads think more than they say.
- **SFX: 2-5 per page**, romaji + English translation in parens, from the lexicon in `bible/style.md`.
- **At least 3 [CHIBI] panels per chapter**, dropped mid-conversation as comedy beats — never grouped, never announced.
- **Every chapter contains a domestic beat**: food, laundry, a bandage, a bus ride, dishes, a bento, an umbrella. It gets real page time, not a cutaway.
- Fights and magic are seasoning: at most one short System/duel/Tower scene per chapter, and it must pay a character beat. The quiet is the meal.
- **THERE ARE NO SILENT PAGES.** Every page carries dialogue, page 1 included. A quiet moment gets three bubbles, never zero. Do not designate any page as "the silent page."
- **The final page's last panel carries `- CAPTION: CH N — END`** (small box). The linter enforces it.

## Breakdown grammar (strict — scripts parse this)

```markdown
# CHAPTER 4 — TITLE HERE

LOGLINE: one sentence.
CONTINUITY IN: 2-4 bullets of what this chapter inherits from the log.

## PAGE 01
PANEL 1 [big]: Camera + action description, present tense, concrete.
- CAPTION: CHAPTER 4 — TITLE HERE
- (Renka): "SHORT LINE HERE."
- (Renka, thought): "PRIVATE REACTION."
- (Renka, aside): so heavy...
- SFX: GACHA (kachak)
PANEL 2 [small]: ...
- (Sou): "REPLY, 4-8 WORDS."
...

## PAGE 13 — SPLASH
PANEL 1 [splash]: ...
- (Renka): "..."

## CONTINUITY NOTES
- New facts established / promises made / props introduced / relationship deltas.
```

Rules:
- Page headers exactly `## PAGE NN` (two digits), splash `## PAGE NN — SPLASH`.
- Panel lines start `PANEL k [size]` with size ∈ `big | wide | tall | small | splash`; append `[CHIBI]` for chibi panels.
- Dialogue lines: `- (Name): "UPPERCASE TEXT."` · thought: `- (Name, thought):` · aside: `- (Name, aside): lowercase mutter` · `- SFX: ROMAJI (translation)` · `- CAPTION: TEXT`.
- Speaker names must match `bible/cast.md`. Voices must match each character's sample lines.
- End with `## CONTINUITY NOTES` — publish appends these to the log.

## Lint (mechanical, before any drawing)

`pwsh scripts/lint-breakdown.ps1 -Chapter N` checks: page count ≥25; per-page bubble count 8-14 (splash 3-6); no page with 0 bubbles; bubble word count ≤8; panel count 6-8 per page (splash 1); exactly one splash; ≥3 chibi panels; CONTINUITY NOTES present. Violations go back to the writer; drawing never starts on a failing breakdown.

## Calibration gate (every chapter, before the full draw)

Do NOT generate 25 pages and then judge. Generate TWO:
1. **Page 1**, and **the densest mid-chapter conversation page** (most bubbles). The conversation page is the real gate — page 1 alone cannot show whether dialogue density renders right.
2. OPEN the two files and **COUNT**: panels (must be ≥6 and match the script ±1), speech bubbles (≥8 and every scripted bubble present), panels containing a human face (all panels, or all-but-one). Confirm portrait ratio, B&W, screentone.
3. **Zero bubbles or zero faces is a failure to fix, not a variation to report.** Adjust the page prompt emphasis and redraw until both pages pass, then draw the rest.

## Cover (after the chapter's pages exist)

One per chapter. Use the page-prompt template with the COVER OVERRIDE block from `engine/art-spec.md`: both leads split down the middle between public and private selves, one continuous split line passing through both figures, ~40% white, series title + chapter number lettered on. Output `cover.png` → published as `cover.jpg`.

The writer MAY include a `## COVER NOTES` section (2-3 lines of chapter-specific imagery — props, outfits, season) right before CONTINUITY NOTES; the prompt builder folds it into the cover call.

## Continuity

- The writer reads `continuity/log.md` IN FULL before writing; nothing may contradict it.
- CONTINUITY IS A FILE, NEVER A CONTEXT WINDOW. Every cycle starts cold from bible + log + manifest.
- Publish appends the breakdown's CONTINUITY NOTES to the log with date + chapter number.
- When chapter 12's notes land, the season arc is complete: the next cycle FIRST has the writer append a SEASON 2 outline to `bible/arc.md` (same format, 12 chapters), then continues. The series never ends.
