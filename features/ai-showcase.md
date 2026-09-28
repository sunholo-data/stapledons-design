# AI showcase: a game that talks, remembers and grows its own assets

**Status:** Planned (direction ratified: ledger D-7, 2026-09-28)
**Priority:** P0: the conversations in R1 M4 depend on it, and it is a
flagship for AILANG's AI stack.
**Pillars:** The Ship Is Home (++), Time Has Emotional Weight (++), Grounded
Strangeness (+), The Game Doesn't Judge (+, which constrains every prompt).
**Related:**
- `features/future/dialogue-system.md` (8 emotions, speakers);
- `features/future/archive-system.md` (an NPC whose memory degrades);
- `features/future/narrative-orchestrator.md`;
- `features/future/dialogue-tts-voices.md`;
- `reference/ai-capabilities.md` (Gemini text, image and TTS with emotion
  markers);
- `art/characters-blender-brief.md` (illustrated portraits and static mini avatars) and
  `art/generation-cast-plan.md` (the founding community, households, succession);
- ADR 0001 (the sim runs as a child process; AI calls never go inside the tick).

---

## 1. The idea

The ship carries an AI, so in this game **AI generation is diegetic**. The
Archive (the ship's mind, fused into the spire's base) and the crew speak,
remember, change and appear through generated text, voice and imagery. **The
game also builds its own asset library as it is played.** Every new person born
aboard, every aged face, every alien and every place is generated the first
time it's needed, then cached and reused. No two voyages have the same faces.
The game is its own AI harness.

## 2. What the player experiences

| Feature | What happens | AILANG capability | Milestone |
|---|---|---|---|
| **Talking crew** | Dialogue is generated in character (OCEAN personality, mood, relationships, memories) and **spoken aloud** in each person's own voice | `std/ai` (streaming, tool loops), `std/audio` (TTS) | R1 M4 |
| **Emotion markers** | Generated lines carry markers (the design's 8 emotions); the **portrait changes** and the **voice's delivery follows**, both driven by the same marker | TTS emotion markers (`reference/ai-capabilities.md` §4.2) | R1 M4 |
| **Portraits that age** | Each character's illustrated portraits in 8 emotions, at the life stages the story reaches; later ages are generated **from the character's own reference image**, so they stay the same person. **Descendants are new identities**, never aged-down parents (`art/generation-cast-plan.md`) | `std/ai.callImage` plus image editing from a reference | R1 M4 → R2 |
| **Crew who remember** | Past conversations are embedded; each crew member **recalls** relevant memories ("you said that before we left Sol…") | `std/embedding`, `std/sharedindex` | R2 (a stub in M4) |
| **The Archive's degrading memory** | Implemented *literally*: the Archive's retrieval index loses and blurs entries as its hidden Memory Health falls, so it genuinely misremembers | `std/sharedindex` with a controlled decay | R2 |
| **News from home** | After a long journey, the Earth-side simulation's events are narrated as **generated news**, letters and archive fragments | `std/ai` over the simulation's state | R1 M4 |
| **First contact** | Alien speech with a real **communication-quality** channel: low quality garbles, loses or mistranslates meaning | `std/ai` with degradation tools | R2 |
| **Talk to your ship** | Optional **live voice** with the Archive, where the player speaks and it answers | `sunholo/gemini_live` | R2 stretch |
| **Narrative orchestrator** | A hidden agent reads the simulation and shapes pacing and arcs through tool calls, without railroading | `std/ai.runTools` | R2 |
| **Epilogue** | The Year-1,000,000 legacy is written from the full history | `std/ai` | R3 |

## 3. The asset library that grows (the "harness")

- **One canonical reference per character.** Its first generation produces a
  **character sheet**: face and body reference, palette, and voice choice.
  Every later asset (emotions, ages, figure sprites) is generated **from that
  reference**, never from scratch, so the character stays consistent.
- **Cache key:** `(kind, entity_id, emotion, age_stage, variant)`, together
  with the prompt, the model id, the seed where the model supports one, and a
  content hash. The cache is stored with the voyage's auto-save.
- **Semantic reuse.** Before generating, look up a *near* match in the semantic
  index: a background crew member "tired, age 40, engineering" can reuse an
  existing face. That saves cost and makes the population feel continuous.
- **Quality gate.** Every generated image is checked by a vision call against
  the character sheet (same person? style on-brief? no sky or stars painted in?)
  before it's accepted. A rejection regenerates it, at most 2 retries, then
  falls back to the nearest cached asset.
- **Pre-generated at build time:** the accepted founding cast (the 11 featured
  adults and the recurring children from `art/generation-cast-plan.md`), the
  Archive's presence states, and the captain's UI silhouette.
- **Generated during play:** everyone born aboard, age stages as years pass,
  aliens, civilization scenes, and news imagery.

## 4. In-world figures: static mini avatars (accepted direction)

In the isometric play areas, the crew appear as **static mini avatars** (see
`art/characters-blender-brief.md` §3.1), **seen living their lives**: the
simulation places people at consoles, in gardens, in doorways, and the engine
moves or swaps the avatar. There's **no character animation**. Expression lives
in the large portraits, which swap by emotion marker. The player **selects**
someone to open a conversation (large portrait and voice).

- New people (births, successors) get a **new identity**, a reference portrait
  and a matching avatar, generated when the story needs them and then cached.
- The captain has a static mini avatar and a UI silhouette, and never an
  expressive portrait.

## 5. Architecture

```
Godot (render, UI, audio playback)
  │  NDJSON                       │ requests / results (async)
  ▼                               ▼
AILANG sim (pure, deterministic)  AILANG AI service (separate process: std/ai, std/audio,
  emits AI REQUESTS as outputs      std/embedding, std/sharedindex; gemini_live optional)
  (e.g. dialogue(context))          streams text with emotion markers → portrait and voice
  consumes AI RESULTS as inputs     writes every output to the RECORDING and the asset cache
```

- **The sim never calls AI.** It emits requests. Godot relays them to the AI
  service, and the results come back as **inputs** on a later tick.
- **Replay rule (bar clauses 2 and 4, byte-identical).** Every AI result
  (text, marker sequence, chosen asset id, audio id) is **recorded** in the
  session log. A replay feeds the recording and makes no AI calls. AILANG's
  `stepWithStreamRecorded` and `--emit-trace` / `ailang dev replay` exist for
  this.
- **Latency:**
  - Text **streams**, so the portrait switches as markers arrive.
  - Voice is generated per line, and the next line's voice is prefetched while
    the current one plays.
  - Cached portraits appear instantly. A new generation shows the nearest
    cached face until it's ready.
- **Offline and failure:** the pre-generated core plus the cache keeps the game
  playable without a network. If the AI is unavailable, fall back to templated
  lines, flagged in the log.

## 6. Guardrails

- **The game doesn't judge:** prompts forbid moralising and verdicts. The
  characters have opinions; the narrator doesn't.
- **Canon consistency:** every prompt includes the relevant canon snippets (ship,
  physics, the crew's history). The Archive **never** states physics that
  contradicts `physics/relativity-spec.md`.
- **Content safety:** use the provider's safety filters, plus the quality gate
  in §3.
- **Cost:** a budget per session, with reuse through the semantic cache.

## 7. Open questions (for Mark)

1. **Keys and cost for the shipped game:** the developer's key through a proxy,
   the player's own key, or a hybrid (pre-generated core plus opt-in live
   generation)?
2. **Providers:** Gemini for text, image and TTS, as the reference doc assumes?
   Or mix per capability, routed through AILANG's provider support?
3. **Live voice** (talking to the Archive): R2 stretch, or earlier?

## 8. First implementation steps (the loop's design doc should cover)

1. **AI service skeleton:** an AILANG process, an NDJSON request/result
   protocol, recording, and a cache index. Tested headless with `--ai-stub`.
2. **Emotion-marker grammar:** one inline syntax shared by the text generator,
   the TTS and the portrait switcher, plus a parser and tests.
3. **Voice and markers on the accepted cast:** give the accepted illustrated
   Medic a TTS voice, and play one generated line whose emotion markers swap its
   existing portraits in the spike's conversation UI. **Style frame for Mark**
   (the voice and the swap timing; the portrait style is already accepted).
