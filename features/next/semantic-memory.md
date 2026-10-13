# Semantic memory: similar worlds, news that finds you, and a universe that remembers

## Status

- **Status:** Proposed 2026-10-13 (Mark, attended). He asked to explore AILANG's semantic cache, which "we planned to have within the game to help find similar planets, news feed surfacing, etc. We could be generating a lot of data over many runs, and a way some leak into this universe run vs others perhaps." He later added that embeddings will run locally, through Ollama running embeddinggemma-2, and floated in-game security keys that unlock Archive features and may correspond to real API keys.
- **Priority:** P2. Phase S0 is P1, because it is cheap and needs nothing new.
- **Builds on:**
  - [premise-and-loop.md](../../vision/premise-and-loop.md):
    - §4: the loop is the secret.
    - §6: no memory survives; generated content carries over "as variations, not verbatim"; déjà vu.
  - [ai-showcase.md](../ai-showcase.md) §2:
    - crew who remember;
    - the Archive's degrading memory;
    - §3 semantic reuse of assets;
    - the rule that the sim never calls AI.
  - [archive-voice.md](archive-voice.md): Memory Health, the unreliable narrator, planted clues.
  - Game-repo rulings: D-8 (the player's own key), D-20/D-21 (spend), D-67 (in-world voice), D-68 (the loop stays secret).

---

## Game vision alignment

| Pillar | Alignment | Rationale |
|---|---|---|
| **Choices Are Final** | ✅ Supports | What one universe generated can return in the next. A choice outlives its run, as furniture, never as memory. |
| **The Game Doesn't Judge** | ✅ Neutral | Similarity is description ("this world resembles Earth"), never advice. |
| **Time Has Emotional Weight** | ✅ Supports | News surfaces by what it means to you, not by date order; a drought story finds the captain who left farmland behind. |
| **The Ship Is Home** | ✅ Supports | The Archive and the crew recall; the ship accumulates a past. |
| **Grounded Strangeness** | ✅ Core | "Similar worlds" is computed from measured radius, mass and starlight. Strangeness enters only as the echo, and the Archive cannot explain it. |
| **We Are Not Built For This** | ✅ Supports | Recognition without memory is unsettling, and that is the point. |

---

## What AILANG gives us (measured 2026-10-13)

Measured against ailang `dev` 329f4dd (std v0.54.7) and the pinned v0.54.6. The spike is in the game-repo session notes.

| Need | AILANG today | Fit |
|---|---|---|
| Vector maths | `std/embedding`: cosine, euclidean, normalize, f32/f64 codecs. Pure. | ✅ Runs in `sim/core.ail`. **Strict VM output equals the interpreter's** (checked). |
| Text fingerprints | `std/simhash`: simhash and Hamming distance. Pure. | ⚠️ Lexical only. It can't tell "Orbital shipyards launch the second ark" (0.58) from a drought story (0.58) when the query is about drought. |
| Text meaning | `_ollama_embed(model, text)`, effect IO. Local Ollama, `OLLAMA_HOST` honoured. | ✅ `embeddinggemma-2`: 768-d, about 27 ms per text locally. Correct ranking on the drought test. |
| Similarity index | `std/sharedindex` (needs `--caps SharedIndex`): upsert, findSimhash, findByEmbedding, namespaces. `std/sem` frames over it. | ✅ Within one process. **In memory only:** nothing survives a restart (ailang#1796). |
| Deterministic search | `deterministic=true` sorts by (score desc, key asc). | ⚠️ **Only with `maxScan = 0`.** With a scan limit the scanned subset is random (ailang#1795). |
| Persistent store | SQLite backend exists, but only the `ailang cache` CLI uses it. | ❌ Not reachable from programs (ailang#1796). We keep our own file until then. |
| Provider embeddings (Gemini/OpenAI) | CLI-only embedders. | ❌ Not needed: Mark's call is local Ollama. Ask for an upstream `embed` anyway (#1796). |

**Calibration is per model.** The same five headlines score 0.25–0.75 under `embeddinggemma` and 0.68–0.91 under `embeddinggemma-2`. The ranking is the same; the absolute scale differs. Every floor in this doc is stated per model and checked by a fixture test, never hard-coded across models. The game repo's brain hook showed the same thing: a floor calibrated on one corpus misfired on another.

---

## The features

### S0: Similar worlds (pure, in the sim, no AI)

Each body gets a small feature vector:

> ln radius, ln mass / 3, ln insolation / 2

Insolation is L / a², in Earth units. Nearest neighbours by Euclidean distance, with ties broken by name, are deterministic and replayable.

Measured on the Solar System plus TRAPPIST-1:

| Body | Nearest |
|---|---|
| Earth | TRAPPIST-1 e (0.26), Venus (0.33), TRAPPIST-1 d (0.40) |
| TRAPPIST-1 e | Earth, TRAPPIST-1 f |
| Jupiter | Saturn, Neptune |

**Uses:**

- **The Archive and the HUD:** "the most Earth-like world we have charted".
- **Navigation:** "worlds like this one" as a filter over the catalogue.
- **News templates:** "a world much like the one you left".
- **Later:** the planting of clues (archive-voice phase 2).

The sim emits the ranking; Godot only shows it.

**The weights are a design choice.** They live in data with tests, and the "physics in the package" rule applies if any of the formula grows. Insolation comes from the package's luminosity, never recomputed in GDScript.

### S1: News that finds you (AI service, local embeddings)

Every news item (templated or generated) and every crew bio, home-life note and captain's choice is embedded by the AI service through local Ollama `embeddinggemma-2`. When news arrives, the service ranks candidate items by similarity to what this crew cares about, and the sim surfaces the top one.

- **The sim never calls it.** The sim emits a request; the ranked item ids come back as a recorded AI result, so replays stay byte-identical (the existing replay rule).
- **No Ollama, or no model:** fall back to SimHash, or to date order. The indicator says so, out of world. Nothing breaks.
- **Search with `maxScan = 0`** until ailang#1795 is fixed.

### S2: Crew who remember (R2, already planned)

[ai-showcase.md](../ai-showcase.md) "Crew who remember": past conversations are embedded per crew member and recalled within a run. The same index and namespaces apply, one per crew member. **This is run-scoped: memories never cross** (§6).

### S3: The Archive's memory is its index (archive-voice phase 2)

The Archive's retrieval index is real. As Memory Health falls:

- entries are **dropped**: deleted from the index;
- or **blurred**: their embedding is nudged towards a neighbour's, so a query returns the wrong but nearby entry.

The degradation is deterministic, driven by seed and Health, so replays reproduce it. This is "implemented literally", as ai-showcase promised. It also gives the unreliable narrator a mechanism rather than only a prompt.

### S4: A universe that remembers (the cross-run library)

§6 already rules on what carries over:

- **Generated content** carries over: species, portraits, voices, lore, news stories.
- **Memories** never do.

The library is how that happens.

1. **The library.** Every accepted generation is stored with its embedding, kind and a content hash. It lives in a file in the player's data folder, outside any voyage save (for example `user://library/frames.ndjson`). At service start it is loaded into the in-memory index, until AILANG has a persistent backend (#1796).
2. **Retrieve before generating.** The service searches the library before generating. A near match becomes the seed for a **variation**: same species family, different details. The result is cheaper, and familiar things recur on purpose.
3. **The echo.** The Archive may retrieve from the library, but only above a high floor. The floor is the **wall's thickness**, a tunable setting.
   - A hit comes through as the persona's permitted "faint, unexplained sense of recognition", never as a fact.
   - The secret rule (D-68) still holds. The Archive cannot say *why* it recognises something, and the in-world voice check and reply filter still apply.
4. **Replay.** Every library hit is a recorded AI result, so a replay never reads the library. The library can grow between runs and old recordings still reproduce exactly.
5. **New game** keeps the library and clears the voyage. That is the furniture/memory split, made literal. A "forget the universe" control sits in Settings, out of world, for players who want a clean slate.

### S5: Keys as things you find

Mark's idea: security keys found in play unlock Archive features, and some may correspond to real API keys.

**What exists now:**

- **Where keys go:** Settings, out of world, has a key field per provider. **Pasting is allowed.**
- **How it's stored:** the field is masked, and a saved key is written to a mode-0600 file.
- **Environment keys:** a key from the environment is shown as such and never displayed.
- **Hygiene:** key-hygiene tests keep keys out of logs and recordings.
- **Rulings:** D-67 keeps provider names out of the in-world voice, so key entry stays in Settings.

**The hard rule: no real secret ships in the game.** Anything in the build or its data can be extracted. A provider key hidden in a found item would be scraped within days and billed to us. So a found key can't *be* a raw provider key. Three designs respect that:

| Option | What a found key is | Real API behind it | Cost and risk |
|---|---|---|---|
| **(a) Diegetic framing only** | Settings stays where real keys go. In world, the Archive's "deep stores" open when a key is present, with no key text in world. | The player's own key (D-8). | Zero new risk. This is today's behaviour, given a story. |
| **(b) Capability tokens** (recommended now) | A sim-side unlock found in play: a recorded, replayable flag. It gates Archive features such as deeper recall, spire readings and the echo. | Still the player's own key, when the feature needs AI. | Zero secret risk. Fits Choices Are Final: found, never bought. |
| **(c) Sponsored access** (later, needs a ruling) | A found code is exchanged by our small server for a **short-lived, scoped, rate-limited token**. Codes can be published, even as puzzles, and revoked. | Our account, through our proxy. | Real spend, so a D-20 decision. Changes D-8 for those features. Needs abuse limits and a server to run. |

**Recommendation:**

- Build **(b)** with **(a)**'s framing now.
- Keep **(c)** as an R2+ option for showcase features, such as a sponsored Archive voice for players without a key. It must never be built as a key inside the build.

---

## Architecture and determinism

- **`sim/core.ail`:** S0 only. Pure features and distances, so it passes `--strict-bytecode` and parity. It emits ranking fields and AI requests; it never embeds.
- **AI service (`ai/*.ail`):** embeddings (IO), the in-memory index, the library file (FS) and the provider calls. Everything it returns is recorded.
- **Godot:** shows fields and relays requests. It does no similarity maths.
- **Player data:**
  - the library file, kept with the keys and settings;
  - the voyage saves, which never hold library content beyond the recorded results.

## Phases and acceptance (each AC by command, in the game repo's sprint)

| Phase | Ships | Key checks |
|---|---|---|
| S0 | Similar worlds in the sim, shown at the Archive and in the HUD | Fixture rankings asserted; VM equals the interpreter; weights in data |
| S1 | News surfacing (embedder: local Ollama, cloud, or none, falling back to SimHash or date) | Ranking recorded and replay-exact; fallback path tested with no Ollama; floors per model in a fixture |
| S2 | Crew recall, run-scoped | Nothing crosses New game (test) |
| S3 | Archive index decay (with archive-voice phase 2) | Seeded decay reproduces; dropped and blurred entries asserted |
| S4 | Cross-run library and the echo | Library survives New game; replay never reads it; the echo passes the reply filter and the voice check |
| S5 | Capability-token keys (b) | Unlocks recorded; no key text in world; key-hygiene tests extended |

## Upstream (AILANG)

- **#1795:** `maxScan > 0` breaks deterministic search. Use 0 until fixed.
- **#1796:**
  - a persistent SharedIndex/SharedMem backend, so our own file can go;
  - a std `embed` through the configured provider;
  - fix the docs that call SharedMem "persistent".
- **#1797:** a non-exported `--entry main` exits 0 silently (hit while spiking).

## Rulings and open questions

**Ruled (Mark, attended 2026-10-13):**

1. **Embeddings are configurable and optional.** The choices are local Ollama, a cloud embedding service, or none. With none, S1 falls back to SimHash or date order and S4 stores content without vectors. The setting lives in Settings, out of world, beside the AI keys.
2. **The library is per player.** The design stays open to a later cloud service that shares **anonymous** resources across players' universes. So library frames carry no personal data: only generated content, its kind, its hash and its vector. A future shared tier could then be added as a second, read-mostly scope, like the brain's user and project tiers.
3. **Keys: (b) is approved, capability tokens found in play.** It leaves (c), sponsored access through our broker, open as a later option. (c) would still need a spend ruling and amends D-8 for those features.

4. **The wall is fixed.** The echo's floor never changes with the number of crossings. The universe we start in is nothing special: it is one of many. The Archive works that out only as the evidence builds up: replays, plus more content in the library, give it more to recognise.
5. **The Archive is sandboxed and infers the loop itself.** The game never tells the model the lore's mechanics:
   - not the loop;
   - not the reset;
   - not the pocket universe;
   - not the selection.

   It gets only what an AI aboard would have: its persona, its sensors, its entries, and echo hits (library items that resemble the question, shown as recognised without a source). Any conclusion it reaches about other universes is its own inference from that evidence. The reveal of §4 ("works it out again in each iteration") is emergent, not scripted. **It is also a player goal** (Mark): even a player who knows the secret can play to help the Archive work it out, by bringing it spire readings, echo-rich places, half-recognised worlds and memory repairs.

**Consequences for what is built (archive-voice phase 1):**

- **Done in game PR #214:** the cast's `secrets`, which told the model about the loop and hinted at the pocket universe and luck, are replaced by `bounds`: evidence only, inference said as inference, spire faults, an uncertain origin. `make in-world-voice-check` fails if a lore mechanic reaches the model.
- **Ruled (Mark, 2026-10-13): no theory filter.** "If we have proper sandbox do we need any blockages?" With every AI character sandboxed, a line wondering about loops, other universes, luck or a simulated, chosen universe is the speaker's own inference, not a leak. It passes. Still blocked:
  - **frame breaks:** game, player, fiction, language model, the model's maker, its training and prompt, since sandboxing cannot remove what a model learned about being a model;
  - **numbers:** the instruments hold them.

  The static D-68 check on text *we* write (UI, codex, news) stays, because we know the secret. Game PR #219. Watch in playtests for the model reaching for the simulation trope too early; tune with the evidence rule in the prompt, not with a block.
