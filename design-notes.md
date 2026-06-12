# Design Notes — Supplementary Mechanics & Content
*Distilled from an earlier Gemini design session. Engine-specific (Godot/GDScript)
code from that session was dropped; the design content below is engine-agnostic
and carries forward. Where a note conflicts with `gdd.md`, the conflict is flagged
— do not silently resolve.*

---

## 1. Asset & Legal Strategy

- **The "percentage rule" is a myth.** Modifying an NES asset by any amount still
  yields a derivative work. If an average observer says "that's just Link," it's
  infringement. Fair use rarely protects commercial game dev.
- **Hybrid sourcing:**
  - CC0 environment/tile assets: Kenney.nl ("1-Bit Pack", "Micro Roguelike"),
    OpenGameArt (tags: `CC0`, `NES Palette`), itch.io — for walls, floors, pits,
    traps, UI borders.
  - Procedural audio via BFXR/Sfxr for original royalty-free 8-bit SFX
    (Ricoh 2A03 style). *(Note: current prototype already generates audio in
    WebAudio code — arguably better, no files needed.)*
  - Philosophers: start from a CC0 base character template, redraw identity
    features (Sartre's glasses, turtleneck/trenchcoat, pipe) in Aseprite over
    professional walk cycles.

## 2. Nausea Meter System ⚠️ conflicts with GDD §6

GDD treats Nausea as **gateway moments** (screen distortion that hides truths
behind it). This proposal makes it a **persistent stamina-replacement meter**:

- **Builds via:** existential enemy attacks, excessive dodging (dodge-spam = Bad
  Faith), missed swings with cursed weapons, hazard zones.
- **Tiers:** 0–30 normal · 31–70 speed −20%, green vignette · 71–99 speed −50%,
  heavy screen warp.
- **Drains via:** authentic action (landing hits), Black Coffee consumable.
- Loop framing: *Act → bear the weight of the action → manage the dread.*

These could coexist (meter for moment-to-moment, gateways for set pieces) — needs
a decision.

## 3. Combat Loop Alternatives ⚠️ partially conflicts with GDD §3

- **The Pen** — quick close-range thrust (agrees with GDD).
- **The Pipe** — ranged smoke cloud: minimal damage, obscures vision, reveals
  invisible paths, triggers switches. *(GDD's ranged option is The Gaze. Could be
  two separate items with different roles: Gaze = reveal/attack, Pipe = utility.)*
- **The Philosophical Pivot** — dodge with i-frames that slightly raises Nausea
  (avoiding conflict is Bad Faith). Elegant risk/reward; no GDD equivalent.

## 4. Alan Watts — The Ambiguous Merchant (new content)

Shop NPC who sells items priced in **Angst** (currency dropped by enemies) and
describes every item only through Zen parables — **stats are hidden**.

- *The Heavy Blade* — "A farmer once found a wild stallion…" → 2× damage,
  0.8× speed, +15 Nausea on miss.
- *The Empty Flask* — "The usefulness of a cup is in its emptiness…" → catches
  enemy spells/projectiles for re-throw; heals nothing.
- Pattern: every Watts item is a genuine trade-off the player discovers through
  play, never through a stat sheet. Thematically: the determinist tools repurposed
  idea (items.md) meets Watts's non-dualism — he sells you "pieces of the universe."

## 5. Albert Camus & The Boulder — recurring mid-boss (new content)

Camus is **unkillable and unbothered**. He blocks passages while pushing his
boulder; the fight is a **puzzle**: manipulate the environment so the boulder
rolls backward and he must step aside to chase it. Recurs across dungeons with
escalating puzzle complexity. He's friendly — parting gifts after each encounter.

## 6. Dungeon 1: Freud Fight — three-phase design (richer than GDD §5 D1)

1. **The Id:** Freud summons a **Shadow Sartre** that mirrors the player's
   movement through the room's center point (player offset negated). Attacking
   it directly spikes Nausea massively — instead, maneuver it into environmental
   traps.
2. **The Superego:** floating eye fires beams while floor tiles collapse; Pipe
   smoke reveals the invisible safe path.
3. **Freudian Slips:** Freud fires **text projectiles** ("MOTHER", "LIBIDO");
   catch them with the Empty Flask and throw them back.

GDD's D1 (couch trap, free-association chains) can merge with this — couch trap
fits naturally as a Phase 2 or 3 hazard.

## 7. Laplace's Demon — input-prediction implementation note (D9)

The Demon aims projectiles at where you *will* be: read the player's **raw input
vector for the current frame before physics applies it**, compute
`predicted_position = player_pos + input_velocity × projectile_flight_time`, fire
at that point. Dodging requires breaking continuous motion — stutter-stepping,
stopping when prediction expects movement. This is the cheap, honest version of
"the Demon predicts you" and complements the GDD's show-your-button-press-early
presentation.

## 8. Freud follower — breadcrumb pathing note

No pathfinding AI needed: player records a position history ring buffer
(~30 frames); Freud steers toward the oldest entry. Smooth trailing follow that
never gets stuck. *(Current prototype already has a Freud follow implementation —
compare before changing.)*

## 9. Dialogue Quote Bank

**Sartre**
- "Existence precedes essence! I am what I make of myself!"
- "The only neurosis here is your obsession with my childhood! Now, get out of my way." *(to Freud)*
- "My biology wrote the script, but I am the one burning the theater down!" *(to Sapolsky)*

**Freud**
- "You claim you are radically free, Jean-Paul. But your freedom is an illusion sitting on an ocean of repressed desires."
- "A textbook manifestation of the death drive. You are subconsciously punishing yourself."
- "Oh, I am not leaving, Jean-Paul. A good analyst never abandons his patient."

**Alan Watts**
- "Ah, the universe has arrived to purchase a piece of itself. How playful."
- "A fair trade. Though, you know, you never actually possessed that dread in the first place."
- "You are fighting the universe to prove you are separate from it. But a wave does not need to fight the ocean to prove it is water. *[Chuckles]* Yes, I have bombs. Three for 10 Angst."

**Camus**
- "Rebellion is not a sword swing, Jean-Paul."
- "One must imagine me happy!"
- "And your choices are an excuse to avoid enjoying the struggle. Here, take this. I won't be needing it anymore."

**The Determinists**
- Harris: "Notice how you did not author that action. Thoughts simply arise from the void."
- Pereboom: "A manipulated agent feels just as free as you do right now."
- Sapolsky: "See? You were always going to do exactly that. The hormones, the childhood, the genes commanded it."

---

## Dropped from the source session (and why)

- All GDScript code — Godot-specific; project is web-first. Logic preserved above
  in plain language.
- Embedded minimal HTML5 demo of D8 phase mechanics — superseded by the full
  `index.html` prototype.
- "Engine: Godot 4" framing — superseded by the web-first decision in `CLAUDE.md`.
