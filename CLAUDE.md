# CLAUDE.md — Sartre's Quest: The Dungeons of Determinism

8-bit Zelda-style action-adventure. The player is Jean-Paul Sartre fighting through
9 dungeons, each themed around a determinist thinker. Freedom vs. determinism is the
theme AND the mechanic. Full design in `docs/gdd.md`; items in `docs/items.md`;
supplementary mechanics and content in `docs/design-notes.md`.

## Tech direction (decided)

- **Web-first.** The game must run in a browser. Mobile comes later via Capacitor
  wrap or PWA — do not block on it, but don't make choices that preclude it
  (no desktop-only APIs, touch input must be addable).
- **Stack choice is delegated to you.** Vanilla canvas (current), Phaser, PixiJS,
  Kaplay, or similar are all acceptable. Before any refactor, evaluate against the
  existing prototype and write your decision + reasoning to `docs/adr-001-engine.md`.
  Bias: the prototype is ~2,900 lines of working dependency-free vanilla JS.
  A framework must earn its place; "rewrite because frameworks are nicer" is not
  a reason. "The room/entity/input systems will collapse under 9 dungeons without
  better structure" might be.
- **No build step unless the ADR justifies one.** If you introduce tooling, prefer
  Vite, keep it minimal, and document run commands here.

## Current state

`index.html` is a complete single-file vertical slice of Dungeon 8 (The Laboratory
of Illusions): player movement/combat, tile collision, room transitions, three boss
phases with the control-lying gimmick, Freud companion with commentary, dialogue
system, bitmap font, procedural chiptune-ish audio, game over / victory screens.
It is the canonical baseline. Treat it as load-bearing reference even after
refactoring — behavior parity matters.

## Hard conventions (from the prototype — keep these)

- **Native resolution 256×240** (NES), integer-scaled, `image-rendering: pixelated`,
  no smoothing. Render to an offscreen buffer at native res, then blit scaled.
- **16px tiles, 16×15 grid**, top HUD bar 16px.
- **All colors come from the `PAL` palette object.** No ad-hoc hex values in
  gameplay code. Extending the palette is allowed; bypassing it is not.
- **Bitmap pixel font + `drawText`/`wrapText`** for all text. No DOM text, no
  web fonts in the play area.
- **Audio is procedurally generated WebAudio** (square/triangle/saw, 2A03 vibe).
  No audio file dependencies unless the ADR changes this.
- Sprites are defined as paletted string data (`parseSprite`) — keep new sprites
  in this format unless/until an asset pipeline is introduced deliberately.

## Design rules

- Determinists are **not straw men**. Boss dialogue presents their arguments with
  real force. Sartre wins by fighting for freedom, not by the opponent being dumb.
- Items and mechanics are **philosophical arguments made tangible** — check
  `docs/items.md` before inventing new pickups.
- Freud commentary is frequent, funny, and occasionally genuinely useful.
- Facticity: never let the player transcend all limits. Constraints are the point.

## Open design questions (do not silently resolve — raise with the developer)

1. **Nausea:** gateway moments (GDD §6) vs. persistent stamina-style meter
   (`docs/design-notes.md`). Possibly both. Needs a decision.
2. **Ranged weapon:** The Gaze (GDD) vs. Pipe smoke-cloud utility shot
   (design-notes). Could be two items; could be one.
3. Scope of the next milestone — see `docs/` for backlog; confirm with the
   developer before starting multi-session work.

## Working agreements

- Commit early, commit small, message format: `area: what changed`.
- Behavior-change refactors and feature work go in separate commits.
- When adding a dungeon mechanic, cite the GDD/design-notes section it implements
  in the commit body.
- Playable in browser at every commit on `main`. No broken checkpoints.
