# Earth's Fate on the Home Page: the doom clock as difficulty, and fate-dependent Home pictures

> **SPOILER: story and endgame.** This document describes how Earth ends. Nothing in it, and none
> of its art, goes on the website, in News posts, in release notes or in public captions (see
> [Spoilers](#7-spoilers-and-the-reference-art)).

## Status

- **Status:** Future. **Unscheduled.** No release or sprint owns it yet.
- **Rulings:** Mark, attended 2026-10-19 (game ledger **D-87**).
- **Builds on:**
  - [vision/premise-and-loop.md](../../vision/premise-and-loop.md) §1 (the physics of the threat), §2 (the three clocks), §3 (paths to saving Earth), §4 (the spire seals the run), §8–9;
  - [endgame-legacy.md](endgame-legacy.md) ("Earth's fate is always shown at the end");
  - [black-holes.md](black-holes.md) (the rogue black hole as a scenario object);
  - the game's **R2-HOME-TIERS**: stapledons-godot `design_docs/planned/r2/home-tiers.md` (on branch `design/home-tiers` until it lands on main; ledger D-86). Its canon note says tiers 13–15 (10,000 years and later) show Earth's fate, ship as a "no threat modelled yet" baseline, and wait for this design.
- **Reference art:** private (§7). The first, illustrated set at `gs://stapledons-voyage-dev-builds/reference/earth-fate/` is **superseded** by the art direction in §4a; the rendered set (with `notes.md`) is at `gs://stapledons-voyage-dev-builds/reference/earth-fate/rendered/`.

## The rulings (Mark, attended 2026-10-19)

Mark's words, verbatim, answering questions about Earth's doom and the Home pictures:

> "and canonically in our story, when does the blackhole destory earth? how much time has it left? we could even have a variation of the doom if the ship is uncuccessful"

> "yeah lets have them so we can reference them later. lets make it a difficulty setting how much time we have left."

- **R1. Earth's remaining time N is a difficulty setting**, inside the canon range of about 10,000 to 24,000 Earth years until the rogue black hole's pass. The levels in §1 are *proposed*, not ruled.
- **R2. The crossing review's Home pictures become fate-dependent** past the point where the doom matters, with a **pool of picture variants per fate**: approaching, saved, scorched, ejected, captured, comet showers, diaspora (§4).

## Game vision alignment

Scored by hand against [core-pillars.md](../../vision/core-pillars.md); score again with `game-vision-designer` before a sprint.

| Pillar | Alignment | Rationale |
|---|---|---|
| **Choices Are Final** | ✅ Core | N is chosen once, at new game, and fixed for the run. A crossing that runs past N lands on a fate the player cannot undo |
| **The Game Doesn't Judge** | ✅ Supports | The pictures report what Earth became, never a score. "Saved" is a picture too, not a victory screen. Risk: an "ejected" picture can read as blame; the lines stay factual and conditional |
| **Time Has Emotional Weight** | ✅ Core | Today the deep-time pictures say "a long time passed". With this, a long crossing can come back to arks on the launch towers, or a dark sky. The doom stops being a title card and becomes something the player sees the cost of |
| **The Ship Is Home** | ⚪ Neutral | About Earth, not the ship. The crew's reaction belongs to the crew systems |
| **Grounded Strangeness** | ✅ Core | Every outcome follows the tidal-kick physics in premise §1: eccentric orbit, ejection, capture, and comet showers delayed by Oort-cloud infall times |
| **We Are Not Built For This** | ✅ Core | Deep time with a deadline: the horror of returning to an Earth that ended while you crossed |

## 1. The Earth clock as a difficulty setting (R1)

N is chosen at new game, shown on the title card ("You have 100 years. The Earth has N."), and fixed for the run. It counts Earth years from the ship's emergence at the black hole, so the opening flight home is already spent from it (premise §1).

**Proposed levels (open: OQ1).**

| Level | N (Earth years) | Light-year budget (premise §2) | Black hole distance today at 45 / 100 / 300 km/s |
|---|---|---|---|
| Gentle | 24,000 | total path ≤ ~24,000 ly | 3.6 / 8.0 / 24 ly |
| **Standard** (default) | 16,000 | total path ≤ ~16,000 ly | 2.4 / 5.3 / 16 ly |
| Hard | 10,000 | total path ≤ ~10,000 ly | 1.5 / 3.3 / 10 ly |

The distances are speed × N (45 km/s = 1.50 × 10⁻⁴ c). The black hole's mass and speed stay tunable with N (premise §9); the difficulty picks N and the scenario derives the rest.

Rules:

- **Determinism.** N is an input to world generation, recorded with the seed. The same seed and N give the same run.
- **The doom moment is a sim value.** `doom_year = N` on the Earth clock. The sim owns it; the host never computes it.
- **Not a slider mid-run.** Changing N would rewrite what the player already chose; it is fixed at new game.
- **The loop (premise §4).** Each run is a variation of the universe; whether the variations may also vary N (the AI steering towards kinder doom clocks) is OQ2.

## 2. How the sim decides the fate state

Let **t** be Earth years since the run began (the sim's Earth clock; the crossing review's `cruise.digest.time.earth_year_total` counts from departure and is converted by the sim). The fate state is a pure function of the seed, N, t, the salvation and evacuation state, and the ship's distance from Sol. It has no hidden state.

```
fate(t) =
  Baseline       if t < F                       -- before the fate window opens (§3)
  Approaching    if F ≤ t < N                   -- Earth knows; arks and launch towers
  outcome(t)     if t ≥ N
```

`outcome(t)` is decided **at N** from the state of the run then, and never changes afterwards except for the late comet-shower stage:

1. **Saved**, only if a salvation path *completed* before N: move the Sun, move Earth, or counter the black hole (premise §3 paths 1–3), including "Earth solved it without you" when the sim models it. Nothing else gives Saved.
2. **Diaspora**, if no salvation completed but an evacuation path completed (premise §3 path 4, or "fail but seed a diaspora"): the people left; the pictures show the empty, overgrown Earth they left behind. (Whether Diaspora also shows the physical outcome after N is OQ4.)
3. **Captured**, if the Sargasso path completed (premise §3 path 5): Earth in orbit around the black hole, kept alive.
4. Otherwise the **physical outcome**, from the pass distance q (a hidden world-gen draw from the seed, shifted by any partial "counter the black hole" progress) through the tidal kick Δv/v of premise §1:
   - Δv/v below the habitability limit (~10 %): **Scorched**, an eccentric orbit and climate catastrophe;
   - above it, or q within a few AU (chaotic): **Ejected** or **Captured**, chosen deterministically from the seed. A Captured Earth without the Sargasso path is a dead one; its pictures differ from the Sargasso pool only if OQ5 says so.
5. **Comet showers (late stage).** A pass through the Oort cloud sends comets inward long afterwards: infall from ~10⁴ AU takes a large fraction of an orbital period, hundreds of thousands of years. From **t ≥ N + S** (S proposed at 200,000 years, tunable), an Earth still bound to the Sun (Saved, Scorched or Diaspora) shows the comet-shower pool. Ejected and Captured Earths are no longer in the inner Solar System and never do.

**What the ship can know (D-68).** The Home page must never show what the ship cannot know. The outcome at N is visible to the ship only once light from Sol at N can have reached it:

```
shown_fate(t) = fate(t)            if t ≥ N + d_ship(t)   -- d in light-years, Earth frame
              = Approaching        otherwise (for t ≥ F)
```

The same gate applies to the comet-shower stage (N + S + d). The sim computes both; the host only reads `fate`. Whether the Archive's line hedges differently when the ship has not yet seen the outcome is part of the lines work (§5).

This fate value is also what [endgame-legacy.md](endgame-legacy.md) shows at the end, and is a candidate for the spire's "sealed" trigger (premise §4, §9). This design does not resolve "sealed"; it gives it a sim value to use.

## 3. How the Home pictures branch by years and fate

R2-HOME-TIERS gives each crossing a tier from Earth years (16 tiers, 0–15) and each tier a pool of variants picked per voyage from the seed (D-86). This design adds a second axis on top of it.

- **Before the fate window (t < F):** the tier picture, exactly as R2-HOME-TIERS ships it. Nothing changes.
- **From the fate window on (t ≥ F):** the picture comes from the **fate pool** of `shown_fate(t)`, not the tier pool. The tier still sets the line's time words and the narration key.
- **Where the window opens (proposed; OQ3):** `F = min(10,000, N − 3,000)`. That is the canon floor already used by the home-tiers canon note (tiers 13–15), pulled earlier on Hard so that Hard still has an approaching stage: Gentle and Standard open at 10,000 years, Hard at 7,000 (inside tier 12).
- **Today's tiers 13–15 pictures become the Saved pool** (§4). They were drawn as "no threat modelled", which is what a saved Earth looks like in deep time.
- **The pick.** Same rule as D-86, with its own salt: `fateVariant(seed, fate, tier) = top53(splitmix64(fateKey(seed), fate × 16 + tier)) mod count(fate)`. It draws nothing from the world's `Rng`. One voyage, fate and tier always give the same picture; different tiers inside one fate can give different pictures.
- **Older streams.** Streams below the protocol minor that adds `time.fate` show the tier picture, byte-identical to today.

## 4. The variant pools

Each fate has a pool, `fate_<name>_v<k>.webp`, counted in a data module like `home_variants.ail`, so adding a variant is data only.

| Fate | When (§2) | What the pictures show |
|---|---|---|
| **approaching** | F ≤ t < N, or after N until the outcome's light reaches the ship | Earth knows. Arks under construction, launch towers on the skyline, a world organised around leaving or holding on. The sky is still the normal sky: the hole is invisible to the eye (§4b) |
| **saved** | outcome at N, a salvation path completed | Today's deep-time pictures (tiers 13–15): ice ages, moved coasts, the long ordinary future. The "no threat modelled" baseline |
| **scorched** | outcome at N, moderate kick | An eccentric orbit and climate catastrophe: at a 0.5 AU perihelion the Sun is 1.07° across and 4× as bright over burned land |
| **ejected** | outcome at N, strong or chaotic kick | Eternal night and ice. The Sun is a dazzling point, not a disc: magnitude −16.7 at 100 AU (still brighter than the full Moon), −11.7 at 1,000 AU, and no warmth. No aurora |
| **captured** | outcome at N: the Sargasso path, or a chaotic capture | Earth in orbit around the black hole (the Sargasso ending). The hole has no visible disc; it shows as lensed stars, an Einstein ring at most about 0.57° across at the closest survivable orbit (~2.4 × 10⁶ km, beyond the fluid Roche limit) and a few arcminutes at AU distances (4.3′ at 1 AU). The shadow (≤ ~13″) is invisible to the eye. The Sun is far off |
| **comet showers** | t ≥ N + S, Earth still bound to the Sun | The Oort cloud's long aftermath as a *rate*: a bright comet every few years, not a sky full; old impact scars |
| **diaspora** | evacuation completed, no salvation | An empty, overgrown town. People left |

Reference art for every pool is generated to §4a and §4b (§7). Shipped pictures will be chosen from it the way D-86 chose the tier variants.

### 4a. Art direction: rendered skies, photographic Earth (Mark, attended 2026-10-19)

Mark, verbatim: "ok lets make them more accurate not illustrations". Asked what that meant, he chose: **the sky is rendered by the game's own physics renderer** (the real star catalogue, the Schwarzschild lensing shader, the Sun's true size and brightness for each orbit), with the landscape composited underneath.

Mark, verbatim: "I think its going to be terrifying to see real earth but under skys only explored via the space ship, cold or hot etc."

- **Fate pictures are photographic.** Realistic Earth landscapes, not the illustrated house style of the Home tiers.
- **The style break is the point.** Every Home picture before the doom is illustrated. When the fate arrives the style breaks into photographic reality, under a sky the player has only ever seen from the ship.
- **The sky is not painted.** It comes from the same renderer as the ship's view, so it obeys the physics spec and its check values: the star catalogue, the lensing shader, and the Sun's angular size and magnitude computed for the fate's orbit (§4b). A fate sky that the renderer cannot produce is a renderer gap, not an art choice.
- **Approaching and saved** keep a normal sky; the doom is not visible from Earth (§4b). Whether approaching and saved also switch to photographic is OQ8.

### 4b. The physics of the fate skies

For the 10 M☉ rogue hole of premise §1: Schwarzschild radius r_s = 29.5 km; shadow radius (3√3/2) r_s = 76.7 km.

| Viewer's distance from the hole | Einstein ring diameter (background stars) | Shadow |
|---|---|---|
| 3 ly | 0.6″ | invisible |
| 1,000 AU | 8″ | invisible |
| 100 AU | 26″ | invisible |
| 10 AU | 82″ | invisible |
| 1 AU | 4.3′ | invisible |
| Closest survivable captured orbit (~2.4 × 10⁶ km, ~81,000 r_s) | ~0.57° | ~13″: invisible to the eye |
| Earth's rigid Roche limit (~1.2 × 10⁶ km; not survivable, for reference) | ~0.8° | ~26″: still invisible to the eye |

The ring diameter is 2√(2 r_s / D) for a source far behind the hole; the shadow diameter is 2 × 76.7 km / D.

**Roche limits for Earth around a 10 M☉ hole** (M/m ≈ 3.3 × 10⁶): rigid d ≈ 1.26 R⊕ (M/m)^⅓ ≈ 1.2 × 10⁶ km; fluid d ≈ 2.44 R⊕ (M/m)^⅓ ≈ 2.3 × 10⁶ km. Earth is closer to the fluid case, so **a survivable captured orbit needs r ≳ 2.4 × 10⁶ km** (about 81,000 r_s). Even there the hole's shadow is about 13″, invisible to the eye: a captured sky shows lensed stars, never a black disc. (Corrected 2026-10-19 by the render spike; the first draft used the rigid limit.)

- **The hole is invisible to the naked eye on approach.** Earth detects it by astrometric microlensing of background stars, by drift in the outer planets' orbits and by a disturbed comet cloud. That is how OGLE-2011-BLG-0462 was found. It fits the canon (premise §1): Earth cannot see the doom until the ship tells it where to look.
- **Ejected:** the Sun is magnitude −16.7 at 100 AU and −11.7 at 1,000 AU (−26.7 + 5 log₁₀ d). There is no aurora.
- **Scorched:** a perihelion of 0.5 AU makes the Sun 1.07° across (twice today's 0.53°) and 4× as bright.
- **Comet showers** raise the rate of bright comets to about one every few years. Not a sky full.
- These values go into the physics spec as check values, with tests, before any fate sky ships (the game's definition of done, gate 2).

### 4c. Renderer prerequisites before fate art ships

The rendered reference set (`gs://stapledons-voyage-dev-builds/reference/earth-fate/rendered/`, with its `notes.md`) found four gaps in the game's tools. Each must be closed, with tests, before §4a fate art can ship:

1. **No atmosphere model.** The game renders space skies only: no day or twilight sky, airglow, scattering or refraction. Every fate sky is seen from the ground.
2. **The sim refuses `bh_r` above 1e6 r_s.** Fate skies need the hole at up to AU distances and beyond (1 AU ≈ 5 × 10⁶ r_s), so the accepted range must grow while the lensing lookup tables stay finite across it (definition of done, gate 5).
3. **The display floor hides near-threshold stars against a bright sky.** With a bright Sun or a twilight sky, stars near the visibility threshold vanish; the floor must account for sky brightness.
4. **Precession and proper motion over millennia are not applied.** A sky 10,000+ years on must move the pole and the stars (the catalogue's proper motions).

## 5. Lines and voice

- The Home line keeps R2-HOME-TIERS' time sentence and adds a fate clause only where the ship can know it (§2). Lines report, never grade (The Game Doesn't Judge).
- D-21: no digits; years only through the sim's `%s`. D-67: in-world voice. D-68: never more than the ship knows.
- The Archive's crossing line gets the fate as context, as a span-of-time word plus a fate word that names a state, not a scene that the picture has not shown.
- Writing the lines is part of the implementing design, not this one.

## 6. The game repo's implementation (when it is scheduled)

A game design doc (`design_docs/planned/<release>/earth-fate-home.md`) will specify, with command-checkable acceptance criteria:

- the difficulty choice at new game, N in the world-gen inputs, and a test that the same seed and N replay byte-identically;
- a pure, strict-VM `fate` function and its table test (VM/interpreter parity), covering every branch of §2 including the light-delay gate;
- `time.fate` (and the fate variant) on the wire at a new protocol minor, with older streams unchanged;
- the host's fallback: fate picture, else the tier picture, else words only;
- `make home-art-check` extended to the fate pools;
- the four renderer prerequisites of §4c, each closed with a test, and the §4b values as physics-spec check values;
- a check that no fate art or fate line reaches `website/`.

## 7. Spoilers and the reference art

- **Fate art never goes on the website**, in News posts, release notes, social posts or public captions. This follows the website spoiler policy (stapledons-godot `website/news/README.md`: never endings, the long-horizon endgame, or what happens to civilisations). R2-HOME-TIERS already keeps tiers 13–15 off the site for the same reason.
- **The reference art is private**, in the private dev-builds bucket, not the public assets bucket. The first, illustrated set at `gs://stapledons-voyage-dev-builds/reference/earth-fate/` is superseded by §4a; the rendered set (with `notes.md`) is at `gs://stapledons-voyage-dev-builds/reference/earth-fate/rendered/`.
- Shipping art is a separate question (OQ6): D-18 puts game masters in the public bucket `gs://stapledons-voyage-assets`, where anyone could find them.

## Open questions

| # | Question | Proposal |
|---|---|---|
| OQ1 | The difficulty levels and their numbers | Gentle 24,000, Standard 16,000 (default), Hard 10,000. Mark to confirm; playtesting tunes them |
| OQ2 | May the loop's variations vary N between runs (the AI steering towards kinder clocks), or does N stay the player's choice? | N stays the player's choice; the loop varies the black hole's pass distance q instead |
| OQ3 | Where the fate window opens | `F = min(10,000, N − 3,000)`. Should early tiers also hint that Earth knows (arks begun centuries in)? Proposal: not before F |
| OQ4 | Diaspora after N: keep the empty-town pool, or switch to the physical outcome's pool? | Keep diaspora until the comet-shower stage; the epilogue reports the physical outcome |
| OQ5 | A dead captured Earth (no Sargasso path) vs the Sargasso ending: one pool or two? | One pool for now; split if the reference art reads differently |
| OQ6 | Where shipped fate art lives, given D-18's public bucket | A private or obfuscated path for spoiler art, or accept that game files are discoverable and keep only the website clean |
| OQ7 | The comet-shower delay S | 200,000 years after N, tunable; check against an Oort infall estimate before shipping |
| OQ8 | Do approaching and saved (normal skies) also switch to photographic, or stay illustrated so that only a doom fate breaks the style? | Stay illustrated: the break then marks the doom itself |
