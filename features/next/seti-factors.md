# SETI factors: what the life model leaves out, and what could honestly be added

## Status

- **Status:** Proposed 2026-11-12. A research review and a proposal. Not approved. It changes no baseline value, no check value and no code. Every number computed here is an estimate and is labelled; none is a check value yet.
- **Origin:** the R3-WORLDS calibration found that giving a player 5 or more living contacts per voyage needs about 10⁵ times baseline luck, with civilisations lasting 32 million years. Mark (attended, 2026-11-12): this fine-tuning is "exactly a deep lesson of the game"; he wants the numbers right, asks what factors the model is missing and what could honestly be added, and wants it to teach SETI. Mark, same day, on the encounter targets: "if we can move the expected numbers a bit that's ok if it makes it easier - but this is all good research on its own."
- **Priority:** P1 for the rulings it asks for (§5.6), because they decide what the default preset means. The model changes are P2.
- **Roadmap:** [r3-signals-and-contact.md](../../roadmap/r3-signals-and-contact.md), tracks W and S.
- **Builds on:**
  - [life-and-intelligence-parameters.md](life-and-intelligence-parameters.md) (the baseline, the dial, the encounter targets, D-144);
  - [physics/life-and-worlds.md](../../physics/life-and-worlds.md) (the normative factor table and the LW check values; this document copies its labels: Verified, Adopted, Design, unverified);
  - [signals-and-instruments.md](signals-and-instruments.md) §4 and [physics/observation.md](../../physics/observation.md) (instruments, technosignatures, the OB check values);
  - [civ-mirror.md](civ-mirror.md) and [inner-shapes-outer.md](inner-shapes-outer.md) (a civilisation as a simulation);
  - [galaxy-beyond-measurement.md](galaxy-beyond-measurement.md) (the play area) and [dark-lenses.md](dark-lenses.md);
  - the `sunholo/worlds` 0.2.0 package (`history`, `dial`, `baseline`), read only.

---

## Game vision alignment

Checked against [core-pillars.md](../../vision/core-pillars.md):

| Pillar | Alignment | Rationale |
|---|---|---|
| **Choices Are Final** | ✅ Supports | Nothing here adds a preview or an undo. Several factors make evidence older and less certain, which gives a commitment more weight. |
| **The Game Doesn't Judge** | ✅ Core | The review's main product is a luck number that is true: which factors it multiplies, from which baseline, and how improbable it is. |
| **Time Has Emotional Weight** | ⚠️ At risk today | With all the luck in a 32-million-year lifetime, no civilisation the player meets will end inside the game (§3.4). The pillar says civilisations "age, change, forget you, or die while you travel". The review proposes a way to get that back at the same luck. |
| **The Ship Is Home** | N/A | No change. |
| **Grounded Strangeness** | ✅ Core | "Drake equation and real science inform the number and distribution." Every factor is tied to a source that was opened, or is labelled as not opened. |
| **We Are Not Built For This** | ✅ Supports | The literature's steady message is that most of what is out there is old, quiet or already gone. |

**Needs a ruling:** four suggestions touch the rule "the anthropic dial never makes a quiet civilisation loud". They are flagged where they occur and collected in §5.5.

---

## 1. Summary

1. **Most of the 10⁵ is paying for a small search volume, not for a missing SETI factor.** The count of civilisations scales with the number of stars searched. The 81.5 ly map holds about 5,000 stars. The mission range in the galaxy document is 500 to 3,000 ly, which holds 10⁶ to 10⁸. Counting over 1,000 ly instead of 81.5 ly removes about 3 orders of magnitude of required luck before detection losses (§2.3). This is geometry, and it is the largest term.
2. **Detection, not existence, is the second bottleneck, and the dial may not touch it.** Honest reach for an Earth-like civilisation is light-years to tens of light-years for everything except a radar beam, and a radar beam covers about 5 × 10⁻⁸ of the sky (§3.1). Drake's single factor f_c hides this. The modern replacement is a ladder of technosignature classes, each with its own reach, duty cycle and lifetime.
3. **Where the luck sits matters as much as how much there is.** The same 10⁵ can be one 32-million-year civilisation per planet, or 320 successive civilisations of 100,000 years each on that planet. The number alive at any instant is the same. The second universe has about 320 times more ruins and its civilisations can die while the player is away (§3.4). The current generator cannot express it, because it allows one lineage per planet.
4. **A fixed lifetime of 32 million years with no spreading between stars contradicts the settlement literature.** Hart (1975), Newman and Sagan (1981) and Carroll-Nellenback et al. (2019) differ on much, but all three put the time for a spreading civilisation to reach its neighbours at well under 32 million years (§3.3).
5. **"10⁵ times luckier" is true only against the median.** Under the game's own prior the calibrated universe is about a 1-in-3,500 draw, and it sits about 2.5 orders of magnitude above the prior's mean, not 5 (§3.8). Both statements are true. The Archive should say which one it means.
6. **Our real searches would probably not have noticed the game's lucky universe.** In the calibrated universe about 0.13 % of stars host a living technological world. Breakthrough Listen's first 692 stars would have contained about one, and only one in ten of those is radar class (§3.9). That is a lesson the game can teach with its own numbers.

The ranked recommendations are in §5.1. The proposal for adjusted encounter targets is in §5.3.

---

## 2. How the count works today

### 2.1 The model

From `sunholo/worlds` 0.2.0 and [life-and-worlds.md](../../physics/life-and-worlds.md):

- Habitable-zone worlds per star: q = 0.20 × 0.4 + 0.75 × 0.16 = **0.20** (LW-3 to LW-6). Three fifths of these (0.12 of 0.20) orbit M dwarfs and are labelled "habitability debated".
- A habitable world gets life with probability fLife, then at most **one** technological lineage with probability fMinds × fTech. Its onset is uniform over a 5 Gyr window centred on now. It lasts a **fixed** lifetime L.
- It ends as a remnant (75 %), ecocide (15 %) or transcended (10 %). A remnant's traces persist for a log-uniform time between 10³ and 10⁷ years, then fade.
- The dial multiplies fLife, fMinds, fTech and L by powers of ten. Fractions saturate at 1. Luck is the gain in the product after saturation.

### 2.2 The counting identity

For any search, the expected number of living technological worlds inside radius R is

```
alive(R) = N★(R) × q × A,      A = fLife × fMinds × fTech × L / T_window
```

N★(R) is the disc star count (LW-23 to LW-27). A is the fraction of habitable-zone worlds that hold a living technological lineage now.

| Quantity | Baseline (zero dial) | Calibrated |
|---|---|---|
| fLife × fMinds × fTech | 0.632 × 0.0316 × 0.1 = 2.0 × 10⁻³ | 1 × 1 × 1 (all saturated): +2.70 dex |
| L | 10⁵ yr | 3.2 × 10⁷ yr: +2.51 dex |
| A | 4.0 × 10⁻⁸ | 6.4 × 10⁻³ |
| Luck | 0 | **5.2 dex** |
| Living technological worlds per star (q × A) | 8.0 × 10⁻⁹ | 1.28 × 10⁻³ (0.13 %) |

"Dex" means orders of magnitude: 5.2 dex is a factor of 10^5.2.

The number of signals detected is the same identity with two more factors per detection channel: the fraction of living worlds that emit in that channel, and the volume within which the ship can detect it.

```
signals = Σ over channels:  N★(R_channel) × q × A × f_channel × (duty × beam fraction)
```

Remnants use the trace time instead of L: the fraction of habitable-zone worlds in a remnant state is fLife × fMinds × fTech × 0.75 × (mean trace time) / T_window. The mean of a log-uniform trace time between 10³ and 10⁷ years is 1.09 × 10⁶ years. At saturation that gives **1.6 × 10⁻⁴**, and it does not depend on L at all.

### 2.3 Reconstruction: the luck needed against the radius searched

This is my reconstruction, not the calibration's output. It asks: how much luck gives an expected C living technological worlds inside radius R, if every one of them could be found?

```
luck (dex) = log10( C / (N★(R) × q) ) − log10(4.0 × 10⁻⁸)
```

| R (ly) | N★(R) | Habitable-zone worlds | Luck for C = 5 | Luck for C = 2 | Lifetime needed if the fractions are saturated (C = 5) |
|---|---|---|---|---|---|
| 81.5 (the map) | 5.0 × 10³ | 1.0 × 10³ | **5.1 dex** | 4.7 | 2.5 × 10⁷ yr |
| 326 (GCNS limit) | 2.9 × 10⁵ | 5.9 × 10⁴ | 3.3 | 2.9 | 4.3 × 10⁵ yr |
| 500 | 9.9 × 10⁵ | 2.0 × 10⁵ | 2.8 | 2.4 | 1.3 × 10⁵ yr |
| 1,000 | 6.7 × 10⁶ | 1.3 × 10⁶ | 2.0 | 1.6 | 1.9 × 10⁴ yr |
| 3,000 | 1.0 × 10⁸ | 2.1 × 10⁷ | 0.8 | 0.4 | 1.2 × 10³ yr |

The first row reproduces the calibration: 5.1 dex and a lifetime of 25 million years, against its reported 10⁵ and 32 million. So the calibration is, in effect, asking for five living civilisations inside the 81.5 ly map. The calibration also reports that most of its civilisations sit beyond that map. Both are consistent with the identity: at the calibrated values there are about 6 living worlds inside 81.5 ly and about 8,600 inside 1,000 ly.

The table assumes perfect detection. Real detection gives some of the saving back (§3.1). The calibration's own output should replace this table.

---

## 3. Factor review

Each factor has the same eight parts: what it is; sources; the literature's range; how it would enter the model; the effect, with arithmetic; whether it reduces the luck needed or redistributes it; what it teaches; the risk. Source labels:

- **Verified (text):** I opened the paper's full text and read the passage.
- **Verified (abstract):** I opened the abstract only. Claims are limited to what the abstract says.
- **Secondary:** I confirmed the claim only in another source, which is named.
- **Unverified:** I could not open it. The claim is from memory or from a search summary and must be checked before use.

The full source ledger is in §6.

### 3.0 The search volume (added: the largest term)

**What it is.** The number of anything in a search scales with the number of stars searched. Near the Sun that is the cube of the radius, until the search is taller than the disc (scale height 300 pc, LW-2).

**Sources.** Internal. [galaxy-beyond-measurement.md](galaxy-beyond-measurement.md) problem 4: "the mission-relevant play area is about a 500–3,000 ly sphere around Sol". The life feature's decision 1 counts encounters "inside the doom window and range". LW-23 to LW-27.

**Range.** 81.5 ly (5 × 10³ stars) to 3,000 ly (10⁸ stars).

**How it would enter.** No new physics. The calibration's reference strategies would be evaluated over the range the design already names, which needs the 326 ly to 2 kpc completion band and synthesised planets beyond the measured core.

**Effect.** From §2.3: 81.5 ly to 326 ly is a factor of 58 in stars, **−1.8 dex**. To 1,000 ly it is a factor of 1,340, **−3.1 dex**. Detection losses give some back: if only radar-class worlds can be found at those distances (one in ten today), add 1 dex. Net **−0.8 to −2.1 dex**.

**Reduces or redistributes?** Reduces. It is the only item in this review that removes whole orders of magnitude without a new assumption.

**Teaches.** Rarity is a statement about volume. The same universe is "empty" inside 80 ly and "crowded" inside 3,000 ly. This is the point the life feature already makes about time dilation: range and time make rarity playable.

**Risk.** Travel cost. Five contacts spread through a 1,000 ly sphere are about 900 ly apart, so a chain of five costs about 4,500 ly of path against a 16,000-year Earth clock. That is affordable, but each one costs centuries of home's time, where a neighbour at 40 ly costs decades. It also depends on starmap work that is planned but not built.

### 3.1 Detectability against existence

**What it is.** Drake's f_c is "the fraction of civilisations that release detectable signs". It is one number standing for four separate questions: what kind of sign, how far it carries, what fraction of the time it points at you, and for how long the civilisation emits it. Modern SETI replaces f_c with a list of **technosignatures**: any observable evidence of technology. A **beacon** is a deliberate signal. **Leakage** is an accidental one. **Duty cycle** is the fraction of time a signal is on. **Beam solid angle** is the patch of sky a transmitter covers.

**Sources.**
- NASA Technosignatures Workshop Participants (2018), [arXiv:1812.08681](https://arxiv.org/abs/1812.08681), eds. Gelino and Wright. Verified (abstract): it is the report of the September 2018 Houston workshop; its Section 2 covers "current limits on technosignatures". I did not open the body.
- Socas-Navarro et al. (2021), "Concepts for future missions to search for technosignatures", Acta Astronautica, [arXiv:2103.01536](https://arxiv.org/abs/2103.01536). Verified (text), §2: the **ichnoscale** ι "is defined as the relative size" of a technosignature's footprint "in units of current Earth technology" (Fig. 1 caption). The paper plots ι against the number of targets, and says "we might expect that civilizations with a lower ichnoscale would be more abundant". It adds that ι need not grow linearly with a civilisation's age and "may grow exponentially in some cases".
- Sheikh et al. (2025), "Earth Detecting Earth", AJ 169, 118, [arXiv:2502.02614](https://arxiv.org/abs/2502.02614). Verified (abstract): Earth's signatures "span 13 orders of magnitude in detectability", and intermittent planetary radar beats "its nearest non-radio competitor by a factor of 10³ in detection distance". The observation registry already verified its §2 numbers (OB-33).
- Forgan and Nichol (2011), Int. J. Astrobiology, [arXiv:1007.0850](https://arxiv.org/abs/1007.0850). Verified (abstract): an SKA-like instrument could detect human-like leakage to about 100 pc; if civilisations are "radio loud" for about 100 years, the chance of two accidentally detecting each other is about 10⁻⁷.
- Saide, Garrett and Heeralall-Issur (2023), MNRAS, [arXiv:2304.13779](https://arxiv.org/abs/2304.13779). Verified (abstract): peak power leaking from mobile towers is about 4 GW, and it "could not currently be detected" within 10 ly with a Green Bank class telescope.
- Frank Drake, quoted in E. Quill, "Can you hear me now?", [Science News, 9 April 2010](https://www.sciencenews.org/article/can-you-hear-me-now). Verified (text): "Our improving technology is causing the Earth to become less visible." The same article's caption says San Francisco's Sutro Tower radiated less than half its former power after the 2009 digital switch.
- Benford, Benford and Benford (2010), "Searching for cost-optimized interstellar beacons", [arXiv:0810.3966](https://arxiv.org/abs/0810.3966). Verified (abstract) only for its subject: beacons built to be seen over thousands of light-years, designed for least cost. The claim that such beacons would be narrow, pulsed and revisit a target rarely is from my reading of the paper and is **unverified** here.
- Wright et al. (2022), "The case for technosignatures", ApJL 927, L30, [arXiv:2203.10899](https://arxiv.org/abs/2203.10899). Verified (abstract): technosignatures "could be more abundant, longer-lived, more detectable, and less ambiguous than biosignatures"; "technology can outlive its creators and even its host star".

**Range.** There is no agreed value. The literature agrees on the structure and disagrees on the direction. Drake and Forgan and Nichol argue that the leakage phase is short and fading. Saide et al. answer that mobile and other systems partly replace broadcast power, though still below what a nearby observer could detect. Wright et al. (2022) argue the opposite of Drake for technosignatures as a whole. The **radio-quiet trend** is therefore real for broadcast leakage and unsettled overall.

**How it would enter.** As **detection factors**, never dialled. Replace the two leakage classes with a short ladder of technosignature classes. Each class has: an emitted quantity, a reach from the OB photon or link budget, a duty cycle and beam fraction, a prevalence among living worlds, and a persistence after the civilisation ends (§3.2). The prevalences have no measurement behind them and must be labelled Design.

**Effect.** Three pieces of arithmetic.

1. *The radar beam.* The registry's radar has EIRP 2 × 10¹³ W (OB-30). If the transmitter is about 1 MW (Arecibo's S-band transmitter; **unverified**, from memory), the antenna gain is 2 × 10⁷, so the beam covers 1/(2 × 10⁷) = **5 × 10⁻⁸ of the sky**. A single such radar is seen from a given star for that fraction of its pointing time. The registry already says the beam, not the budget, hides a radar. The size of the effect is 7 dex per radar, before counting how many radars a world runs and where they point.
2. *Atmospheric and surface signs.* The coronagraph's inner working angle limits an Earth-twin to 59 ly (OB-24). Inside 60 ly there are about 2,000 stars and 400 habitable-zone worlds. At the calibrated A that is 2.6 living technological worlds. So even if every living world showed an industrial gas at a detectable level, this channel adds about 2 or 3 signals at 5.2 dex of luck, and almost none at lower luck.
3. *Against the radar class.* Radar-class worlds (one in ten) inside the 1-second hint range of 168 ly (OB-51) number 4.4 × 10⁴ stars × 0.2 × 6.4 × 10⁻³ × 0.1 = 5.6 at the calibrated values, before the beam factor. That matches the "5–10 signals" target, which suggests the calibration does not yet charge the beam factor in full. That needs checking against its output.

Net: adding the gas and surface classes raises signals by about 0.15 dex (2.6 more on a base of 5.6). The beam factor, charged honestly, lowers radar signals by several dex unless a world runs many radars or a beacon.

**Reduces or redistributes?** Neither, on balance. It makes the count honest. It is likely to *raise* the luck the signals target needs, unless age-dependent loudness is allowed (below) or the target is moved (§5.3).

**⚠ Rule flag (1 of 4): loudness that depends on age.** A civilisation that lasts 32 million years is assumed today to leak like Earth in the year 2000 for all of that time. The literature points the other way in both directions: Drake says leakage fades within a century; Socas-Navarro et al. say footprint may grow with age. A fixed rule "older civilisations are drawn from larger ichnoscale classes" would be a Design prior that is not dialled. But the dial moves lifetime, lifetime moves age, and age would then move loudness. That is the dial making civilisations loud at one remove. It needs an explicit ruling. The strict alternative is to keep loudness independent of age and accept short detection horizons.

**Teaches.** Why "are they there?" and "can we tell?" are different questions. Why Earth's own radio brightness peaked within a human lifetime. Why the strongest thing we emit, a radar beam, is also the least likely to be seen.

**Risk.** Honesty: the class prevalences are invented, and a table of invented numbers can look measured. Each must carry the Design label in the Archive. Complexity: moderate; the instruments feature already lists the classes. Pillars: none at risk if loudness stays undialled.

### 3.2 Signatures that outlast their makers

**What it is.** The time a sign remains detectable need not equal the time its makers survive. A long-lived gas, a structure in orbit, or a shell around a star can outlast a civilisation by thousands to billions of years. This is the natural route to **remnants that can be found from a distance**, which the model lacks: a dead civilisation has no detector signature today, and the ship cannot land (D-11), so remote sensing is the only way to find one.

**Sources.**
- Schmidt and Frank (2019), "The Silurian hypothesis", Int. J. Astrobiology, [arXiv:1804.03748](https://arxiv.org/abs/1804.03748). Verified (text), §1: the oldest extant large-scale land surface is about 1.8 million years old; ocean sediment exists only for periods after about 170 million years ago, because ocean crust recycles; urban area is "less than 1% of the Earth's surface"; "for potential civilizations older than about 4 Ma, the chances of finding direct evidence of their existence via objects or fossilized examples of their population is small". §2: "the longer a civilization lasts, the more sustainable its practices would need to have become in order to survive", and so "the smaller the footprint". §2: plutonium-244 (half-life 80.8 million years) and curium-247 (15 million years) would persist. PCBs last "many hundreds of years in river sediment". All of these are *in situ* geological traces.
- Schwieterman et al. (2024), "Artificial greenhouse gases as exoplanet technosignatures", ApJ 969, 20, [arXiv:2405.11149](https://arxiv.org/abs/2405.11149). Verified (text): fluorinated gases have "atmospheric lifetimes of ∼2000–50,000 yr" (citing Mühle et al. 2010, not opened); abstract: detectable on TRAPPIST-1f at S/N ≥ 5 in 25, 10 or 5 transits with JWST MIRI/LRS for the three concentrations modelled.
- Haqq-Misra et al. (2022), PSJ 3, 60, [arXiv:2202.05858](https://arxiv.org/abs/2202.05858). Verified (abstract): CFCs at one to five times Earth's level are detectable on TRAPPIST-1e in about 100 hours only under an optimistic 10 ppm noise floor, and not at all under a conservative 50 ppm floor.
- Stevens, Forgan and O'Malley-James (2016), "Observational signatures of self-destructive civilisations", Int. J. Astrobiology, [arXiv:1507.08530](https://arxiv.org/abs/1507.08530). Verified (abstract): each self-destruction scenario leaves an observable signature, but "the decay of signatures is rapid compared to humanity's communication lifetime".
- Benford (2019), "Looking for lurkers", [arXiv:1903.09582](https://arxiv.org/abs/1903.09582). Verified (abstract): Earth's co-orbital objects are "an attractive location" for a probe sent to watch Earth, and have had no SETI or radar study.
- Wright (2018), "Prior indigenous technological species", Int. J. Astrobiology, [arXiv:1704.07263](https://arxiv.org/abs/1704.07263). Verified (abstract): remaining indigenous technosignatures in the Solar System "might be expected to be extremely old".
- Socas-Navarro (2018), "The Clarke exobelt", ApJ, [arXiv:1802.07723](https://arxiv.org/abs/1802.07723). Verified (abstract): a belt of geostationary satellites with face-on opacity about 10⁻⁴ "would be easily observable" in a transit light curve.
- Dyson (1960), Science 131, 1667. **Unverified** (not opened).
- Wright et al. (2014), the Ĝ search, papers [I](https://arxiv.org/abs/1408.1133) (Verified, abstract) and [II](https://arxiv.org/abs/1408.1134) (verified in the observation registry, OB-38 to OB-40). Griffith et al. (2015), paper III, ApJS 217, 25, [arXiv:1504.03418](https://arxiv.org/abs/1504.03418). Verified (abstract): of about 10⁵ galaxies, none reprocesses more than 85 % of its starlight into the mid-infrared, and 50 are consistent with more than 50 %.
- Suazo et al. (2024), Project Hephaistos II, MNRAS, [arXiv:2405.02927](https://arxiv.org/abs/2405.02927). Verified (abstract): 7 candidates from about 5 million objects, all M dwarfs. Ren, Garrett and Siemion (2024), [arXiv:2405.14921](https://arxiv.org/abs/2405.14921). Verified (abstract): three of the seven are contaminated by background dust-obscured galaxies and "have therefore been misidentified"; the authors suggest the same may explain all seven.

**Range.** Per class, with the persistence after the makers end:

| Class | Persistence after the end | Remote reach | Status |
|---|---|---|---|
| Radio leakage, beacons | none (stops with the power) | OB-31 to OB-37 | Verified budgets |
| Short-lived industrial gas (NO₂) | days to years | tens of ly | Design; residence time not sourced here |
| Fluorinated gases | 2,000 to 50,000 yr | tens of ly | Verified (Schwieterman) |
| Night-side lights | none | very close, or the lens | Design |
| Structures on an active planet | "small" chance of direct evidence beyond about 4 million yr; less than 1 % of the surface to begin with | the lens only | Verified (Schmidt and Frank) for Earth |
| Orbital belts, artefacts on airless bodies, probes | unknown; argued to be very long | transits; the lens | Unverified as numbers |
| Stellar-scale structures | unknown; a shell that still absorbs starlight still re-radiates it, with nobody home | across the galaxy (OB-40) | Design; no measured case |

The literature disagrees about the last two rows. Wright et al. (2022) argue technology can outlive its star. Schmidt and Frank argue that a long-lived civilisation leaves a *smaller* footprint. Real surveys have found no confirmed stellar-scale structure in about 5 million stars or 10⁵ galaxies.

**How it would enter.** A **detection factor** plus one change to the history generator's output. Today `tracesUntilT` is a single time. It would become a persistence per class, each drawn inside the class's range, with the class set fixed at the lineage's birth. Remnant persistence is not dialled today and must stay that way.

**Effect.** At saturated fractions, 1.6 × 10⁻⁴ of habitable-zone worlds are in a remnant state (§2.2). The target is 3 to 6 remnants.

- Inside the 81.5 ly map: 1,000 worlds × 1.6 × 10⁻⁴ = **0.16 remnants**. The target cannot be met there at any detection efficiency.
- Meeting 3 to 6 needs 19,000 to 37,000 habitable-zone worlds: every remnant found out to **about 230 to 270 ly**.
- A fluorinated gas lasts at most 5 × 10⁴ yr of a trace time that averages 10⁶. The mean time it stays visible is 3.4 × 10⁴ yr, so it marks 5 × 10⁻⁶ of habitable-zone worlds: 0.002 inside 60 ly. **Gases do not make remnants findable** in this model.
- Structures do, if they exist: a stellar-scale structure is visible across the galaxy (OB-40). If a fraction f_K of ended civilisations left one that lasts the full trace time, the count inside 1,000 ly is 1.3 × 10⁶ × 1.6 × 10⁻⁴ × f_K = 220 × f_K. A target of 3 to 6 needs f_K of about 2 %.
- With several lineages per planet (§3.4), remnant worlds become 30 to 300 times more common, and the target is met inside the map without any megastructure.

**Reduces or redistributes?** It does not change the luck for living contacts. It makes the remnant target reachable, which today it is not.

**⚠ Rule flag (2 of 4).** The game's rule says the dial never changes remnant persistence. Nothing here does. But a megastructure prevalence f_K that is tied to lifetime (old civilisations build, young ones do not) would let the lifetime dial raise the number of loud remnants. Keep f_K independent of the dialled lifetime, or rule on it.

**Teaches.** The Silurian question: if a civilisation had lived on Earth 50 million years ago, could we tell? Why "absence of evidence" on a geologically active planet is weak. Why a dead Dyson shell still glows in the infrared. Why the 2024 Dyson candidates turned out to be background galaxies.

**Risk.** Honesty: f_K is pure Design, and the real surveys' answer is "none found". The Archive must say so. Pillars: megastructures are spectacular and could crowd out the quieter ruins the vision describes. Complexity: low for the per-class persistence, since the classes already exist in the instruments feature.

### 3.3 Settlement and expansion

**What it is.** A civilisation that sends ships or probes to nearby stars becomes many inhabited systems with one origin. The number of occupied systems then depends on how fast and how far it spreads and how long each settlement lasts, not on how often minds arise.

**Sources.**
- Hart (1975), QJRAS 16, 128. Verified (text) from a [reprint](https://www.astro.ncu.edu.tw/~wchen/FindLife/mhart.htm) with OCR errors, not checked against the journal: "Fact A", "There are no intelligent beings from outer space on Earth now"; at 10 % of light speed "most of our Galaxy would be traversed within 650,000 years"; conclusion, "we are the first civilization in our Galaxy".
- Tipler (1980), "Extraterrestrial intelligent beings do not exist", QJRAS 21, 267. **Unverified**: I could not open it. Secondary sources say it argues that self-replicating probes could cover the galaxy in under 300 million years.
- Newman and Sagan (1981), Icarus 46, 293. Verified (abstract, [NASA NTRS record](https://ntrs.nasa.gov/citations/19810061288)): with population growth and saturation, a colonisation front from the nearest independent civilisation reaches Earth only if that civilisation lasts longer than 2.6 million years; 20 million without discretisation; 13 billion with zero population growth. Their conclusion: Earth's lack of visitors reflects the number of worlds against plausible lifetimes, not the rarity of spacefarers.
- Gray (2015), "The Fermi paradox is neither Fermi's nor a paradox", Astrobiology 15, 195, [arXiv:1605.09187](https://arxiv.org/abs/1605.09187). Verified (abstract): the argument "appears to originate in Hart's argument, not Fermi's question".
- Carroll-Nellenback, Frank, Wright and Scharf (2019), AJ 158, 117, [arXiv:1902.04450](https://arxiv.org/abs/1902.04450). Verified (text). The **Aurora effect** is their name for "good worlds are hard to find": only a fraction f of systems is settleable (§1). With stellar motions of 30 km/s they find an upper limit on the galactic crossing time of 300 million years, independent of probe speed and range (§4.1). With finite settlement lifetimes the galaxy reaches a steady state with a settled fraction between 0 and 1 (§5, §8.1). Simulations with a 10 ly probe range show "clusters of closely-packed settled systems surrounded by larger unsettled voids" (§8.1), and a solar neighbourhood that can go unvisited for a million years or more inside an inhabited galaxy.
- Zackrisson et al. (2015), ApJ 810, 23, [arXiv:1508.02406](https://arxiv.org/abs/1508.02406). Verified (abstract): from 1,359 disc galaxies, at most 3 % of local discs show galaxy-wide star-shrouding engineering (conservative limit), and a tentative limit of 0.3 %.
- Hanson, Martin, McCarter and Paulson (2021), ApJ, [arXiv:2102.01522](https://arxiv.org/abs/2102.01522). Verified (abstract): in their model "loud" expanding civilisations now control 40 to 50 % of the universe's volume; we might meet them in 200 million to 2 billion years; and for quiet civilisations to have been active in our galaxy, the chance of a quiet one becoming loud must be about 10⁻⁴ or less.
- Sandberg, Drexler and Ord (2018), §4, Verified (text): the lack of visible settlement in our galaxy or others is the observation that moves their estimate most (§3.8).

**Range.** Crossing times from 650,000 years (Hart) to an upper limit of 300 million (Carroll-Nellenback et al.). The steady-state settled fraction is anything between 0 and 1. The literature disagrees on the conclusion: Hart and Tipler read the empty Solar System as proof nobody exists; Newman and Sagan and Carroll-Nellenback et al. show an inhabited galaxy can leave Earth alone.

**How it would enter.** A **structural change** to the history generator: a technological lineage may found daughter settlements on habitable-zone worlds within a probe range, after a delay, each with its own finite lifetime. It needs three new parameters (probe range, settlement delay, settleable fraction) and none of them is measured. They would be Design values, fixed across presets.

**Effect.** If each origin has on average m living daughter settlements, living technological worlds rise by a factor (1 + m) with no change in the origin rate. The luck needed falls by log10(1 + m): **0.6 dex for m = 3, 1.0 dex for m = 9**. Carroll-Nellenback's own example range is 10 ly at 0.01c: a 1,000-year hop. Any lifetime the dial is likely to choose is far longer than that, so m is limited by the settleable fraction and the settlement lifetime, not by time.

The contacts are no longer independent. Five contacts could be one origin and four daughters, clustered within tens of light-years.

**What a settlement front near the Sun would look like.** From Carroll-Nellenback et al.: a cluster of occupied systems a few probe ranges across, a sharp edge, and voids. In the game's terms: many technosignatures with related spectra in one patch of sky, with ages that fall towards the edge.

**The contradiction with today's model.** At the calibrated lifetime of 32 million years, every source above says a spreading civilisation would have reached its neighbours: 32 million years is 50 times Hart's crossing time and 12 times Newman and Sagan's 2.6 million. A universe of 8,600 isolated 32-million-year civilisations inside 1,000 ly is not one the literature describes. Either they spread, or something stops every one of them, and that something is a claim the game would be making.

**Reduces or redistributes?** Reduces the origin rate needed, by up to about 1 dex for modest m. But it does so with a new factor, not one of the four.

**⚠ Rule flag (3 of 4).** D-144 says only life, minds, technology and lifetime are dialled. Settlement would not be dialled. But its effect grows with lifetime, so the lifetime dial would also raise the number of occupied worlds and, through their signs, the number of signals. It adds a fifth uncertain factor to the count. If it is added, the luck number must include its effect or name it separately (§3.8).

**Teaches.** The Fermi question in its real form (Hart's "Fact A"), and the two honest answers to it. Why stellar motion makes "they have not had time" a weak answer. Why the Sun can be in a void.

**Risk.** Honesty: three unmeasured parameters. Pillars: Pillar 5 asks for "maximum diversity" of species; daughters of one origin are one species unless the civ-mirror simulation lets them diverge over the light-lag. Sandberg et al. count the absence of settlement as the strongest evidence that we are alone, so a default universe with a front near the Sun differs from ours in the most visible way possible. Complexity: high (spatial correlation in a generator that today treats each planet alone; the `propagate` kernel assumes independence).

### 3.4 More than one technological species per planet; more than one living world per system

**What it is.** Two separate things. **Re-emergence:** after one technological species ends, another may arise on the same planet later. **Shared origins:** life may be carried between planets of one system on rocks thrown off by impacts (**lithopanspermia**), or between young stars in the cluster where they formed.

**Sources.**
- Wright (2018), above. Verified (abstract). It treats a prior species in the Solar System as an open empirical question.
- Schmidt and Frank (2019), above. Verified (text), §1: complex life has existed on land for about 400 million years, and industrial civilisation for about 300; the paper asks whether an earlier one could be detected, and does not claim one existed.
- Frank and Sullivan (2016), Astrobiology, [arXiv:1510.08837](https://arxiv.org/abs/1510.08837). Verified (abstract): unless the chance that a habitable-zone planet develops a technological species is below about 10⁻²⁴, "humanity is not the only time technological intelligence has evolved" in the observable universe.
- Lingam and Loeb (2017), PNAS 114, 6689, [arXiv:1703.00878](https://arxiv.org/abs/1703.00878). Verified (abstract): panspermia "is potentially orders of magnitude more likely to occur in the TRAPPIST-1 system" than between Earth and Mars.
- Adams and Spergel (2005), Astrobiology, [arXiv:astro-ph/0504648](https://arxiv.org/abs/astro-ph/0504648). Verified (abstract): life-bearing rocks are captured "10 – 16,000 times per cluster" in favourable cases; my reading of the summary is that only a few of those reach a terrestrial planet.
- Snyder-Beattie et al. (2021), below (§3.6), for the remaining habitable time.

**Range.** Re-emergence has no measured rate. Earth gives one technological species in about 400 million years of complex land life, with perhaps a billion years of habitability left (the last figure is **unverified** here). Shared origins: orders of magnitude easier than Earth–Mars in compact systems; a few events per birth cluster.

**How it would enter.**
- *Re-emergence:* a **structural change**. The generator would draw technological onsets as a rate per habitable world per unit time, instead of one yes-or-no draw. The dial would act on the rate. "Fractions saturate at 1" would no longer cap the dial, and the luck formula would count expected lineages per world instead of a probability.
- *Shared origins:* a **baseline physical factor** on life only: a positive correlation between sibling planets' life draws.

**Effect of re-emergence.** At the baseline it changes nothing: the chance of one lineage per world is 0.003, so a second is a 10⁻⁵ event. Its effect appears only where the dial has saturated the fractions. There it gives a free axis at constant luck. Hold A = 6.4 × 10⁻³ (the calibrated value) and trade lifetime against the number of lineages:

| Lifetime L | Lineages per planet in 5 Gyr | One lineage every | Chance a living contact ends within the 16,000-year game | Chance it ends in a 600-year gap between light leaving and the ship arriving | Share of habitable-zone worlds in a remnant state |
|---|---|---|---|---|---|
| 3.2 × 10⁷ yr (today) | 1 | — | 0.05 % | 0.002 % | 0.016 % |
| 10⁶ yr | 32 | 156 million yr | 1.6 % | 0.06 % | 0.5 % |
| 10⁵ yr | 320 | 15.6 million yr | 15 % | 0.6 % | 5 % |
| 10⁴ yr | 3,200 | 1.56 million yr | 80 % | 6 % | 23 % |

All four rows are 5.2 dex of luck and have the same number of living civilisations. They are very different universes. In the first, nothing the player meets will ever die and ruins are 0.16 per map. In the third, one contact in seven ends during the game and there are about 50 ruined worlds inside the map (1,000 × 0.05).

The remnant share uses the 75 % collapse share and the mean trace time, capped at the gap between lineages.

**Effect of shared origins.** fLife is 0.63 at the baseline and 1 when dialled, so the most it can add to technological counts is 0.2 dex, and nothing at the calibrated values. It raises the number of *biospheres per inhabited system*, and it makes them related.

**Reduces or redistributes?** Redistributes. Re-emergence moves luck from lifetime into frequency. It does not lower the total.

**Teaches.** Re-emergence: deep time on one planet; the Silurian question from the inside, since a species 15 million years after the last would find almost no direct trace of it (Schmidt and Frank's 4 million years). Shared origins: a second biosphere in the same system is not independent evidence that life starts easily, a point real astrobiology will face at TRAPPIST-1.

**Risk.** Honesty: 320 technological species on one planet is as extreme as one that lasts 32 million years; both are the dial, and both must be said plainly. A new intelligent species every 15 million years strains biology; the 10⁶-year row (one per 156 million years) sits closer to Earth's one in 400 million. Complexity: high. `history` is one lineage by construction, `propagateExact` is its exact law, and the package states "It is not a model of repeated epochs". Histories would no longer be byte-identical with 0.1.0 and 0.2.0. Pillars: strongly positive for Pillar 3.

### 3.5 The lifetime distribution

**What it is.** The model gives every civilisation in a universe the same lifetime. Real populations vary. If lifetimes vary, the ones alive at any moment are not a fair sample: a civilisation that lasts ten times longer is ten times more likely to be alive when you look. This is **length-biased sampling**.

**Sources.**
- Kipping, Frank and Scharf (2020), "Contact inequality", Int. J. Astrobiology 19, 430, [arXiv:2010.12358](https://arxiv.org/abs/2010.12358). Verified (abstract): "detected intelligences will have a mean age twice that of the underlying" population; "Older intelligences may be rare but they disproportionality contribute to first contacts"; first contact "would likely be one-sided".
- Balbi and Ćirković (2021), "Longevity is the key factor in the search for technosignatures", AJ, [arXiv:2103.02923](https://arxiv.org/abs/2103.02923). Verified (abstract): a technosignature's duration is "almost entirely separable from the age" of its makers, and "only the ones with the longest duration are likely to be detected".
- Grimaldi (2017), Sci. Rep. 7, 46273, [arXiv:1704.04028](https://arxiv.org/abs/1704.04028). Verified (abstract): "the mean number of detectable emitters is less than one even for detection probabilities as large as 50%", whatever the number of emitters. A search summary adds that distributed longevities enter only through their mean; that sentence is **unverified** in the abstract I opened.
- Grimaldi, Marcy, Tellis and Drake (2018), PASP, [arXiv:1802.09399](https://arxiv.org/abs/1802.09399). Verified (abstract): a civilisation that emits for a time L fills a shell of space L light-years thick. The number of such shells crossing Earth equals Drake's N, whether the shells are thinner or thicker than the Galaxy; for L under 100,000 years the signals arriving may come from civilisations already gone.
- Grimaldi and Marcy (2018), "Bayesian approach to SETI", PNAS 115, E9755, [arXiv:1810.01207](https://arxiv.org/abs/1810.01207). Verified (abstract), loosely: finding no signal within about 1,000 ly is still consistent with a probability above 10 % that of order 100 signals of that power are crossing Earth.
- Sandberg, Drexler and Ord (2018), Table 1. Verified (text): L is log-uniform from 100 to 10¹⁰ years. That is uncertainty about one unknown value, not a spread within one universe.

**Range.** Canon: log-uniform 10² to 10⁸ years, median 10⁵ (LW-10, LW-39, LW-40). Sandberg: up to 10¹⁰.

**How it would enter.** Two separate changes, which should not be confused.

1. *A spread inside one universe* (**structural**): each lineage draws its lifetime from a distribution whose scale the dial sets. The count alive depends on the distribution's **mean**. With an exponential spread and the same mean, counts are unchanged and the living are on average twice the typical age.
2. *Which summary of the prior is the baseline* (**bookkeeping**): the baseline uses the median, 10⁵ years. The mean of the same prior is (10⁸ − 10²)/ln(10⁶) = **7.2 × 10⁶ years**, 1.86 dex higher. The same holds for fMinds (mean 0.145, +0.66 dex) and fTech (mean 0.215, +0.33 dex).

**Effect.** Change 1: zero dex on counts at equal mean. It changes who is met: with a log-uniform spread across six decades, the living are spread evenly in L, not in log L, so 99.9 % of the living have lifetimes in the top three decades. Almost every contact is with an ancient; the short-lived majority are met only as ruins.

Change 2: it would lower the *displayed* lifetime offset from 2.5 dex to 0.65 dex. Nothing physical changes. The universe still needs a mean lifetime of 32 million years.

**Reduces or redistributes?** Change 1 redistributes. Change 2 is relabelling and I recommend against using it to shrink the number (§5.2). Its honest use is in how the luck is stated (§3.8).

**Teaches.** Contact inequality: the first neighbour you find is probably far older than you. The shell picture: every signal is a shell of light as thick as its makers' lifetime, and the number crossing you now equals the number alive now. Why longevity, not abundance, decides what a search finds.

**Risk.** Honesty: low for change 1, provided the spread is labelled Design. Complexity: low on its own (one more keyed uniform, and the reserved slot exists), but it changes `propagateExact`. Pillars: positive for "We Are Not Built For This"; a one-sided first contact is the vision's kind of encounter.

### 3.6 The time structure

**What it is.** The model draws each onset uniformly from a 5 Gyr window centred on now. Real onsets depend on when stars and their heavy elements formed, where in the Galaxy a star sits, how often a planet is sterilised, and how long evolution takes. **Metallicity** is a star's content of elements heavier than helium, which planets are built from. The **Galactic Habitable Zone** is the part of the Galaxy with enough metals and few enough sterilising explosions. **Hard steps** are evolutionary transitions so improbable that they usually take longer than a planet stays habitable.

**Sources.**
- Lineweaver (2001), Icarus, [arXiv:astro-ph/0012399](https://arxiv.org/abs/astro-ph/0012399). Verified (abstract): "three quarters of the earth-like planets in the Universe are older than the Earth", on average by 1.8 ± 0.9 billion years.
- Lineweaver, Fenner and Gibson (2004), Science 303, 59, [arXiv:astro-ph/0401024](https://arxiv.org/abs/astro-ph/0401024). Verified (abstract): the zone is "an annular region between 7 and 9 kiloparsecs from the Galactic center that widens with time", of stars formed 8 to 4 billion years ago; "75% of the stars in the GHZ are older than the Sun".
- Gowanlock, Patton and McConnell (2011), Astrobiology, [arXiv:1107.1286](https://arxiv.org/abs/1107.1286). Verified (abstract): about 1.2 % of all stars host a planet that could have supported complex life; about 75 % of those planets are tidally locked; and "The majority of these planets that may support complex life are found towards the inner Galaxy". This disagrees with Lineweaver et al.
- Behroozi and Peeples (2015), MNRAS, [arXiv:1508.01202](https://arxiv.org/abs/1508.01202). Verified (abstract): the Galaxy hosts about 10⁹ Earth-like planets; the Universe will form more than 10 times the planets that now exist; hence "at least a 92% chance that we are not the only civilisation the Universe will ever have".
- Mor et al. (2019), A&A 624, L1, [arXiv:1901.07564](https://arxiv.org/abs/1901.07564). Verified (abstract): star formation in the disc fell from 9–10 to 6–7 billion years ago, then burst 2 to 3 billion years ago.
- Piran and Jimenez (2014), PRL 113, 231102, [arXiv:1409.2506](https://arxiv.org/abs/1409.2506). Verified (abstract): a 50 % chance of a lethal gamma-ray burst at Earth in the last 500 million years; 95 % within 4 kpc of the Galactic centre.
- Carter (1983), Phil. Trans. R. Soc. A 310, 347. Verified (text, abstract page): the record suggests "at least one but probably not more than two links that were highly improbable (a priori) in the available time interval".
- Snyder-Beattie, Sandberg, Drexler and Bonsall (2021), Astrobiology 21, 265, doi:10.1089/ast.2019.2149. Verified (abstract, via Europe PMC): "expected evolutionary transition times likely exceed the lifetime of Earth, perhaps by many orders of magnitude"; a testable prediction is "that planets orbiting M dwarf stars are uninhabitable".
- Mills, Macalady, Frank and Wright (2025), Science Advances 11, eads5698, [arXiv:2408.10293](https://arxiv.org/abs/2408.10293). Verified (abstract): they propose that there are no hard steps, and that humans appeared "on time" once the environment allowed it.
- Kipping (2020), PNAS, [arXiv:2005.09008](https://arxiv.org/abs/2005.09008). Verified (abstract): life's early start favours fast abiogenesis by a Bayes factor of at least 2.8; "a rare-intelligence scenario is slightly favored at 3:2 betting odds".
- Westby and Conselice (2020), ApJ, [arXiv:2004.03968](https://arxiv.org/abs/2004.03968). Verified (abstract): if intelligence takes about 5 Gyr as on Earth and civilisations last 100 years, at least 36 exist in the Galaxy, the nearest about 17,000 ly away.

**Range.** The literature disagrees at the root. Carter and Snyder-Beattie et al.: intelligence is exceptionally rare, and late. Mills et al.: there are no hard steps. Kipping: weak odds for rare intelligence. Lineweaver against Gowanlock on where the habitable planets are.

**Does a uniform onset over 5 Gyr misstate things, and by how much?** What matters for the count alive is the rate of onsets *now*. The uniform window sets it to 1/(5 Gyr) per lineage. Three corrections:

1. *Planet ages.* If onset follows formation by a roughly fixed delay, onsets follow the star formation history. Disc stars formed over about 10 Gyr, not 5, so the rate per lineage now is nearer 1/(8 to 10 Gyr): **−0.2 to −0.3 dex**. The Sun formed between the 6–7 Gyr minimum and the 2–3 Gyr burst (Mor et al.), so it is not at a peak.
2. *Who came first.* Lineweaver's 75 % means most Earth-like planets had a head start of about 2 Gyr. A centred window gives 50 % before and 50 % after. With a fixed delay, the *past* should hold more ended civilisations than the centred window gives, by a factor of order 1.5 (75 % against 50 %): **+0.2 dex on remnants**, with no gain in the living.
3. *Hard steps.* If the chance of success rises steeply with time (as a power of time in the hard-steps model), onsets are skewed late and the rate now is above average. If Mills et al. are right, onset tracks the planet's environment. The sign is unknown.

My estimate is that the uniform window is within about **±0.5 dex** of any of these, which is small beside 5 dex and inside the spread of the literature. It is an estimate, not a result.

Inside the mission range the Galactic radius barely changes, so the Galactic Habitable Zone does not affect local counts. It matters for the Sgr A* pilgrim: Lineweaver et al. and Piran and Jimenez would make the inner 4 kpc nearly barren, and Gowanlock et al. would make it the richest region.

**How it would enter.** A **baseline physical factor**: replace the uniform onset with one weighted by a star formation history, and add a radial factor to the model galaxy.

**Reduces or redistributes?** Neither, within the uncertainty.

**Teaches.** We arrived late in the Galaxy's history but early in the Universe's planet-making. Most worlds like ours are older. The hard-steps argument and its 2025 challenge are a live scientific dispute the Archive can present with both sides.

**Risk.** Complexity for no gain in counts. The honest course is to keep the uniform window, label it Design (as LW-11 does), and carry the time structure as Archive lore and as the pilgrim's radial factor when the model galaxy is built.

### 3.7 Habitats the model may exclude

**What it is.** The model counts rocky planets in the conservative habitable zone of single FGK and M stars. Other places may hold liquid water: oceans under ice, planets in binary systems, and **rogue planets** that orbit no star.

**Sources.**
- Lingam and Loeb (2019), "Subsurface exolife", Int. J. Astrobiology 18, 112, [arXiv:1711.09908](https://arxiv.org/abs/1711.09908). Verified (abstract): planets with subsurface oceans "may outnumber rocky planets in the habitable zone of stars by a few orders of magnitude"; life there "is likely to face challenges".
- Abbot and Switzer (2011), "The Steppenwolf", ApJL 735, L27, [arXiv:1102.1108](https://arxiv.org/abs/1102.1108). Verified (abstract): a rogue planet keeps a liquid ocean under ice if it is about 3.5 Earth masses, or 0.3 with more water or a thick atmosphere; it could be detected only within about 1,000 AU.
- Sumi et al. (2023), AJ, [arXiv:2303.08280](https://arxiv.org/abs/2303.08280). Verified (abstract): about 21 free-floating or very wide-orbit planets per star between 0.33 and 6,660 Earth masses.
- Shields, Ballard and Johnson (2016), Physics Reports, [arXiv:1610.05765](https://arxiv.org/abs/1610.05765). Verified (abstract): "roughly a third" of rocky M-dwarf planets orbit in the habitable zone; the review covers the open climate questions.
- Raghavan et al. (2010), ApJS 190, 1, [arXiv:1007.0414](https://arxiv.org/abs/1007.0414). Verified (abstract): solar-type stars are 56 % single, 33 % double, 8 % triple and 3 % higher order, as observed.
- Snyder-Beattie et al. (2021), above: predicts M-dwarf planets are uninhabitable. Gowanlock et al. (2011), above: 75 % of candidate planets are tidally locked.

**Range and effect.**

| Habitat | Does the model include it? | Adds biospheres? | Detectable from the ship? | Effect on contacts |
|---|---|---|---|---|
| M-dwarf habitable zone | Yes: 60 % of all habitable-zone worlds, "habitability debated" | Yes, if habitable | Yes | If they are *not* habitable, q falls from 0.20 to 0.08: **+0.4 dex** more luck needed |
| Subsurface oceans | No | Possibly orders of magnitude more worlds | No. No atmosphere to read, no surface to image, and the ship cannot land | None. A technological species under ice is not ruled out, but is not argued for in the source |
| Rogue planets | No | Some, if massive enough | No, beyond about 1,000 AU | None |
| Binaries | Unclear in the package; η⊕ is per star | Roughly the 44 % of solar-type systems that are multiple | Yes | At most about ±0.2 dex |

**How it would enter.** M dwarfs are already a ruling (on by default). The others would be **baseline physical factors** that add worlds with no detector signature.

**Reduces or redistributes?** None reduces the luck. The M-dwarf question can only raise it.

**Teaches.** Most of the Galaxy's liquid water may be under ice and invisible to any telescope. The habitable zone is a statement about what we can detect as much as about where life can be. The largest single uncertainty in the count of habitable worlds is whether red dwarfs count.

**Risk.** Adding undetectable biospheres would make the "biospheres detected" target mean less, for no play. Recommend lore only, plus one line in the luck statement that says how much of the count rests on M dwarfs.

### 3.8 The anthropic framing itself

**What it is.** The game's fiction says each run is a universe selected for being rich in minds near Earth. This section asks what the best-known paper on the subject says, how the luck number relates to it, and how to state the number so that it is true.

**Sources.**
- Sandberg, Drexler and Ord (2018), "Dissolving the Fermi paradox", [arXiv:1806.02404](https://arxiv.org/abs/1806.02404). Verified (text):
  - Table 1: R★ log-uniform 1 to 100; f_p and n_e log-uniform 0.1 to 1; f_l from the rate model; f_i log-uniform 0.001 to 1; f_c log-uniform 0.01 to 1; L log-uniform 100 to 10¹⁰ years.
  - §3.2: "The mean for N is very optimistic, at 27 million, but the median is now only 0.32"; "The probability of N < 1 is now 52 %"; "a nearly 38 % credence that N < 10⁻¹⁰, making us alone in the observable universe".
  - §4: "The Fermi observation thus raises our 52% credence for being alone in the galaxy to somewhere between 53% and 99.6%, depending on the type of evidence considered. It likewise raises our 38% credence for being alone in the observable universe to somewhere between 39% and 85%." Searching nearby stars for signals moves the estimate "a few percentage points"; the lack of visible settlement moves it far more.
  - §3.2: the result "can be driven by our claimed uncertainty in fl alone".
- Carter (1983), above: the weak anthropic principle as a warning about selection in what we observe.
- Hanson et al. (2021), above: an explicit observer-selection model (why we find ourselves early).

**What the Sandberg result says.** It does not say we are alone. It says that when each Drake factor is given an honest range, the product has a huge spread: a mean of 27 million civilisations and a median below one. Most of the probability sits at "nobody"; most of the *expected number* sits in a small set of rich universes. An empty sky is then unsurprising, and there is no paradox.

**How the game's luck number relates to it.** Three true statements about the calibrated universe, from the game's own prior (LW-7 to LW-10, with L to 10⁸):

1. **Against the median:** the product fLife × fMinds × fTech × L is 10^5.2 times the product of the factor medians. This is what `luckDex` reports.
2. **Against the mean:** the prior's mean product is about 0.5 × 0.145 × 0.215 × 7.2 × 10⁶ = 1.1 × 10⁵, against 200 for the product of medians. The mean sits 2.75 dex above the median. The calibrated universe is **about 2.5 dex above the mean**.
3. **As a probability:** the calibrated universe needs fLife near 1 (about half of the prior), and fMinds × fTech × L within a factor of 3 of its largest possible value. The chance of that is about 0.5 × (0.5³/6)/(3 × 2 × 6) = 2.9 × 10⁻⁴. A 4-million-draw Monte Carlo of the prior gives 2.9 × 10⁻⁴. So the calibrated universe is **about a 1-in-3,500 draw**.

A fourth, by coincidence: in Sandberg's own units the calibrated universe has N of about 10 × 0.32 × 0.32 × 3.2 × 10⁷ = 3 × 10⁷ civilisations in the Galaxy, which is close to their *mean* of 27 million. The game's default universe is roughly the average universe of that paper, and far from its typical one.

All four are estimates to be pinned by the package before use.

**The luck number is also conditional on things the dial does not touch.** It multiplies four factors only. It is silent about: the search radius (§3.0, worth 3 dex), detectability (§3.1), the M-dwarf ruling (0.4 dex), and the design constants (window, ending shares, trace times). A statement of luck that leaves these out invites the reading that the universe is 10⁵ times richer in *contacts*. It is 10⁵ times richer in *living technological worlds per habitable planet*.

**A wording that is true** (for the Archive; a proposal):

> "Take the middle expert guess for four things: how often life starts, how often it becomes a mind, how often a mind builds technology, and how long that lasts. Multiply them. This universe beats that product by a factor of 160,000. About one universe in 3,500 does. Nothing else was changed: the stars, the planets, the physics of light and the reach of our instruments are as measured."

And the selection effect, said honestly:

> "You are not here because this universe is likely. You are here because the mission is only possible in universes like this one, and those are the ones that get a ship."

That is the weak anthropic principle in Carter's sense: the observation is conditioned on there being an observer in a position to make it. It makes no claim that our real universe is like this. The Archive should say that in our universe the measured part of the equation ends at the planets, and that Sandberg et al. put the chance we are alone in the Galaxy above one half.

**How it would enter.** No model change. A reporting change: `luckDex` gains two companions, the prior percentile and the offset from the prior mean.

**Reduces or redistributes?** Neither. It changes what the number means.

**Risk.** A second and third number could confuse. The percentile is the easiest to grasp and the hardest to misread, so it should lead.

### 3.9 The search itself

**What it is.** What humans have actually done, what it rules out, and what the false alarms taught.

**Sources.**
- Tarter et al. (2010), "SETI turns 50", Proc. SPIE 7819. **Unverified** directly (the abstract page would not open). Secondary: Wright, Kanodia and Lubar describe its claim as "akin to having searched a drinking glass's worth of seawater for evidence of fish in all of Earth's oceans".
- Wright, Kanodia and Lubar (2018), AJ 156, 260, [arXiv:1809.07252](https://arxiv.org/abs/1809.07252). Verified (text, arXiv version): an eight-dimensional haystack; the sum of searches to date "comes out to 6.0 × 10⁻¹⁸" of it; the oceans hold about 1.335 × 10²¹ litres, so the searched part is like "∼8000 l of seawater", "between the volumes of a large hot tub and a small swimming pool".
- Enriquez et al. (2017), ApJ, [arXiv:1709.03491](https://arxiv.org/abs/1709.03491). Verified (abstract): 692 nearby stars, 1.1 to 1.9 GHz; no "high-duty-cycle radio transmitters" at an EIRP of about 10¹³ W; fewer than about 0.1 % of systems within 50 pc have such a transmitter.
- Price et al. (2020), AJ 159, 86, [arXiv:1906.07750](https://arxiv.org/abs/1906.07750). Verified (abstract): 1,327 stars, 1.10 to 3.45 GHz, "zero candidates"; transmitter power limits of 2 × 10¹² W (Green Bank) and 9 × 10¹² W (Parkes).
- Sheikh et al. (2020), AJ 160, 29, [arXiv:2002.06162](https://arxiv.org/abs/2002.06162). Verified (abstract): 20 stars from which Earth is seen to transit, 3.95 to 8.00 GHz; sensitive to 10⁻³ to 0.88 of the Arecibo radar's power; one candidate, traced to human interference.
- Smith et al. (2021), Nature Astronomy 5, 1148, [arXiv:2111.08007](https://arxiv.org/abs/2111.08007), and Sheikh et al. (2021), Nature Astronomy, [arXiv:2111.06350](https://arxiv.org/abs/2111.06350). Verified (abstracts): BLC1, a narrowband signal near 982 MHz seen while observing Proxima Centauri on 29 April 2019; "blc1 is not an extraterrestrial technosignature" but an intermodulation product of local interferers; "dozens of instances" of similar interference were found.
- The Wow! signal (Big Ear, 1977; narrowband, near the hydrogen line, never seen again). The original report is **unverified** here. Méndez, Ortiz Ceballos and Zuluaga (2024), [arXiv:2408.08513](https://arxiv.org/abs/2408.08513), a preprint. Verified (abstract): they propose a sudden brightening of a cold hydrogen cloud, triggered by a transient source. This is a hypothesis, not a settled explanation.
- Tellis and Marcy (2017), AJ, [arXiv:1704.02535](https://arxiv.org/abs/1704.02535). Verified (abstract): Keck spectra of 5,600 stars; no laser lines above 3 kW to 13 MW; this rules out "models of the Milky Way in which over 0.1 percent of warm, Earth-size planets harbor technological civilizations" that beam optical lasers at us.
- Schwartz and Townes (1961), Nature 190, 205 (the first optical SETI proposal). **Unverified** (not opened).

**What the limits mean, and what they do not.** Each is a limit on a *specific transmitter*, pointed at us, on during the minutes we looked. Enriquez et al. limit continuous 10¹³ W transmitters. They say nothing about an Arecibo-class radar that sweeps past once, nothing about anything below about 10¹² W, and nothing about the 6 × 10⁻¹⁸ of the haystack outside the search.

**Effect: the game's universe against the real searches.** In the calibrated universe 0.128 % of stars host a living technological world, and one in ten of those is radar class.

| Real survey | Stars | Living technological worlds expected among them in the game's universe | Radar-class among them |
|---|---|---|---|
| Enriquez et al. 2017 | 692 | 0.89 | 0.09 |
| Price et al. 2020 | 1,327 | 1.7 | 0.17 |
| Tellis and Marcy 2017 | 5,600 | 7.2 | — (optical; needs a laser aimed at us) |

So Breakthrough Listen's published null results would be the *expected* outcome even in a universe 10⁵ times luckier than the median, because nine in ten of the game's civilisations are quiet and a radar is not a continuous transmitter. The Enriquez limit of 0.1 % of stars is, by coincidence, almost exactly the game's 0.128 %.

**How it would enter.** No model change. Three things for play: (a) the ship's own survey should report limits in the real form ("no continuous transmitter above X W in N stars") instead of "nothing found"; (b) false positives for technosignatures, as the evidence ledger already has for biosignatures: a one-off narrowband signal that never repeats, and interference from the ship itself; (c) an optical-pulse channel beside radio.

**Reduces or redistributes?** Neither.

**Teaches.** The haystack: we have searched a hot tub's worth of an ocean. A null result is a statement about one kind of needle. The Wow! signal and BLC1: a single detection is not a discovery, and verification is most of the work.

**Risk.** A false positive that sends the player 300 light-years for nothing is harsh. It is also exactly Pillar 1. The rate must be low and the evidence ledger must have shown the doubt.

---

## 4. Where the literature contradicts the current baseline

| # | The model today | What the literature says | Size |
|---|---|---|---|
| 1 | Lifetime prior tops out at 10⁸ yr (canon) | Sandberg Table 1 uses 10¹⁰ (already recorded as LW-41) | The mean of the prior moves from 7 × 10⁶ to 5 × 10⁸ yr |
| 2 | Every lineage in a universe has the same lifetime; the baseline is the prior's median | Kipping et al. and Balbi and Ćirković treat lifetimes as a population; what is detected is length-biased; counts follow the mean | 1.86 dex between median and mean |
| 3 | Every living world is isolated, even at 32 million years | Hart: 0.65 million years to cross the Galaxy at 0.1c. Newman and Sagan: a front arrives if lifetimes exceed 2.6 to 20 million years. Carroll-Nellenback et al.: at most 300 million years, with clusters and voids | The calibrated lifetime exceeds two of the three thresholds |
| 4 | One technological lineage per planet, ever | No source measures this. Wright (2018) and Schmidt and Frank (2019) treat an earlier species as an open question | Caps the dial at 2.7 dex in the fractions, which forces the rest into lifetime |
| 5 | Onset uniform over 5 Gyr centred on now | Lineweaver: 75 % of Earth-like planets are older than Earth, by 1.8 ± 0.9 Gyr on average. Mor et al.: the star formation history is not flat | About ±0.5 dex (my estimate) |
| 6 | M-dwarf worlds count in full (60 % of habitable-zone worlds) | Snyder-Beattie et al. predict they are uninhabitable; Gowanlock et al. find 75 % of candidates tidally locked; Shields et al. review open questions | 0.4 dex if excluded |
| 7 | A remnant's traces last 10³ to 10⁷ yr and have no detector signature | Schmidt and Frank: direct evidence on an active planet is unlikely beyond about 4 million years, and is *in situ* geology. Stevens et al.: destruction signatures decay fast. Schwieterman et al.: some gases last up to 50,000 yr. Wright et al. (2022): structures may outlast stars | The upper bound is defensible only for structures off the planet; the remote signature is missing |
| 8 | One technological world in ten is "radar class"; the rest are "TV class" | No source gives this split. Sheikh et al. (2025): Earth itself has both, and its signatures span 13 orders of magnitude in detectability | Design value; should be labelled so |
| 9 | Loudness does not change over a 32-million-year life | Drake and Forgan and Nichol: leakage fades in about a century. Socas-Navarro et al.: footprint may grow with age | Direction disputed |
| 10 | No stellar-scale engineering exists | Consistent with the real sky: none in about 10⁵ galaxies or 5 million stars. But the real sky is not a universe with 8,600 ancient civilisations inside 1,000 ly | A design choice that should be stated |

Rows 2, 3 and 4 are the ones that matter for the calibration.

---

## 5. Synthesis

### 5.1 Recommended changes, ranked

| Rank | Change | Kind | Expected effect on required luck | What it costs |
|---|---|---|---|---|
| 1 | **Count encounters over the mission range the design already names** (at least 326 ly; 1,000 ly as the reference), not the 81.5 ly map (§3.0) | Scope of the calibration; no new factor | **−1.8 dex** at 326 ly; **−3.1 dex** at 1,000 ly, with perfect detection. Net of radar-only detection at range (one in ten): **−0.8 to −2.1 dex** | The 326 ly to 2 kpc completion band and synthesised planets beyond the measured core, both planned. Contacts cost centuries of Earth time each instead of decades |
| 2 | **Let a planet host successive technological lineages; let the dial act on a rate** (§3.4) | Structural change to `history` | **0 dex** in total. Moves up to 2.5 dex from lifetime into frequency. Remnant worlds ×32 at L = 10⁶ yr, ×320 at 10⁵ yr. The chance a contact ends during the game rises from 0.05 % to 1.6 % or 15 % | A new generator and a new exact propagation law; histories stop being byte-identical with 0.2.0; the luck formula counts expected lineages instead of saturating |
| 3 | **Replace the two leakage classes with a technosignature ladder that has duty cycle, beam fraction and persistence after the end** (§3.1, §3.2) | Detection factors, not dialled | Signals: about **+0.15 dex** more from gases and surface signs inside 60 ly, and an honest (large, negative) beam factor on radars. Remnants: from *never found* to findable. Luck for living contacts: unchanged | Class prevalences are Design values with no measurement behind them. Moderate work in `sunholo/observe` and the evidence ledger |
| 4 | **Give lifetimes a spread inside each universe** (§3.5) | Structural, small | **0 dex** at equal mean. Almost every living contact is an ancient; the young appear as ruins | One more keyed draw; `propagateExact` changes with it. Best done together with rank 2 |
| 5 | **State the luck three ways: over the median, over the mean, and as a prior percentile** (§3.8) | Reporting | **0 dex.** The same universe reads as "10^5.2 over the median", "10^2.5 over the mean", "1 in 3,500" | Two new package functions and their check values |
| 6 | **Bounded settlement: a lineage may found a few daughter worlds within a probe range** (§3.3) | Structural; a fifth factor, not dialled | **−0.6 dex** for 3 daughters on average; **−1.0 dex** for 9 | Three unmeasured parameters; spatial correlation; contacts no longer independent; breaks the letter of D-144 (flag 3). Recommend as a later, separately ruled option, not part of the default |

Arithmetic for each row is in the section cited. Ranks 1 and 2 together change the picture most: at 1,000 ly with one in ten detectable, about 3 dex of luck suffices for five contacts, and it can sit in frequency so that civilisations are mortal.

### 5.2 Do not add

| Suggestion | Why not |
|---|---|
| Dial loudness, beacon fraction or megastructure prevalence | It is the rule itself: the dial never makes a quiet civilisation loud. It would also let the game buy signals without buying minds, which is the opposite of the lesson |
| Galaxy-wide settlement, or "grabby" expansion at a large fraction of c | It changes the game into a different one: the sky would visibly belong to someone. Sandberg et al. treat the absence of this as the strongest evidence about our own universe |
| Use the prior's mean instead of its median as the baseline so the luck number shrinks | Relabelling. The universe is the same. Use both numbers openly instead (rank 5) |
| Panspermia as a multiplier on civilisation counts | At most 0.2 dex, and zero once the life dial saturates. Keep it as a correlation between sibling biospheres, and as Archive lore |
| Subsurface-ocean and rogue-planet biospheres as countable encounters | The ship cannot detect them. They would inflate "biospheres" with nothing to see or do |
| Real Dyson-sphere candidates as in-game detections | The 2024 candidates are probably background galaxies. They belong in the Archive as a lesson in false positives |
| A Galactic Habitable Zone gradient inside the mission range | No effect within 3,000 ly. It belongs to the model galaxy and the pilgrim |
| Hard steps as a mechanic | The literature is split (Carter and Snyder-Beattie et al. against Mills et al.). It is lore with two sides, not a parameter |
| A non-uniform onset window now | About ±0.5 dex with unknown sign, for real complexity. Keep the window as Design |
| Excluding M dwarfs by default | Already ruled (on, labelled). State its 0.4 dex in the luck statement instead |

### 5.3 Proposed adjusted encounter targets (for Mark's ruling)

**Origin.** Mark, 2026-11-12: "if we can move the expected numbers a bit that's ok if it makes it easier - but this is all good research on its own." The current targets (life feature, decision 1, 2026-10-27) are per typical 100-year voyage: 40–80 biospheres detected, 5–10 visited, 3–6 remnants, 5–10 signals, at least 5 living contacts.

This is offered beside the model changes in §5.1, not in place of them.

**The plain finding first.** Moving the contact target saves little, because luck scales linearly with the count: going from 5 to 3 saves log10(5/3) = 0.22 dex, and from 5 to 2 saves 0.40 dex. The orders of magnitude are in the range (§3.0) and in where the luck sits (§3.4). The targets are worth adjusting mainly because two of them pull against each other and one cannot be met.

**The tension.** "5–10 signals" and "at least 5 living contacts" together require that between half and all of the signals are still alive on arrival. That forces lifetimes far longer than any light-lag, which is what removes "light from the dead" from the game. **The impossibility.** "3–6 remnants" cannot be met inside the 81.5 ly map at any luck, because the remnant share does not depend on lifetime and is 0.16 worlds per map (§3.2).

| Target | Today | Proposed | Reasoning and arithmetic | Luck saved |
|---|---|---|---|---|
| Biospheres detected | 40–80 | **40–80 (keep)** | Inside 60 ly there are about 400 habitable-zone worlds; at the *baseline* fLife of 0.63 that is 250 living worlds. The limit is telescope time (OB-27: 183 hours for a hint at 59 ly), not luck | 0 |
| Biospheres visited | 5–10 | **5–10 (keep)** | Set by the voyage, not the universe | 0 |
| Signals | 5–10 | **5–10, counted across every technosignature class, and including sources that have since ended** | A sign from the dead is still a signal. With the ladder of §3.1 this is what the instruments report anyway | 0 on the dial; removes the pressure on lifetime |
| Living contacts | at least 5 (5–8 typical) | **at least 3 (3–5 typical)** | 0.22 dex by itself. Its real effect is with the next row: 3 of 5–10 signals alive is a 30–60 % conversion, which a universe of mortal civilisations can give | **0.22 dex** |
| Late contacts (new) | — | **1–2: a civilisation that was alive in the light the ship saw and had ended by arrival, or ends during the game** | Needs mortality on the game's timescale: with rank 2 at L = 10⁵ yr, 15 % of contacts end within the 16,000-year clock, so 1 of 5–8 known civilisations. Not reachable at L = 32 million yr (0.05 %) | 0; it is what rank 2 buys |
| Remnants | 3–6 | **2–4, at least 1 visited** | Today: 0.16 per map. With rank 2 at L = 10⁶ yr: 1,000 × 0.005 = 5 remnant worlds inside the map. With rank 1 instead: 3 to 6 need every remnant found out to 230–270 ly, which needs the ladder's long-lived classes. The lower figure allows for imperfect detection | Makes the target reachable; 0 on the dial |
| Range over which all of these are counted | the 81.5 ly map (in effect) | **the mission range: 1,000 ly as the reference, as decision 1 already says** | §2.3 | **3.1 dex** before detection losses; about **2 dex** net |

**Net.** With the proposed set, ranks 1 to 3, and one in ten living worlds detectable at range, my estimate is that the default preset needs **about 2.5 to 3 dex** instead of 5.2: log10(3 / (1.3 × 10⁶ × 0.1)) − log10(4.0 × 10⁻⁸) = 2.8. As a prior percentile that is roughly a 1-in-15 to 1-in-20 universe instead of 1 in 3,500 (a 3-million-draw Monte Carlo of the same prior gives 1 in 14 at 2.5 dex and 1 in 22 at 3.0 dex; to be pinned). It is still a chosen universe. The fine-tuning lesson survives, with a number the player can hold in mind.

If Mark prefers to keep "at least 5", everything above still applies and the estimate becomes 3.0 dex.

### 5.4 Proposed new check-value rows

These are **proposals**. None is in the registry. Each needs its source re-opened at the stated locator, by someone other than the author of this review, before it is pinned. IDs are provisional and continue from LW-45.

| Proposed ID | Quantity | Value | Unit | Source and status |
|---|---|---|---|---|
| LW-46p | Mean of the canon lifetime prior, (10⁸ − 10²)/ln 10⁶ | 7.238 × 10⁶ | yr | Derived from LW-39, LW-40 |
| LW-47p | Mean of the intelligence prior, 0.999/ln 10³ | 0.1446 | — | Derived from Sandberg Table 1 (Verified) |
| LW-48p | Mean of the technology prior, 0.99/ln 10² | 0.2150 | — | Derived from Sandberg Table 1 (Verified) |
| LW-49p | Mean of the remnant-persistence prior, (10⁷ − 10³)/ln 10⁴ | 1.086 × 10⁶ | yr | Derived from LW-37, LW-38 (Design) |
| LW-50p | Remnant-state share of habitable-zone worlds at saturated fractions, 0.75 × LW-49p / LW-11 | 1.63 × 10⁻⁴ | — | Derived |
| LW-51p | Baseline living share A₀ = LW-7 × LW-8 × LW-9 × LW-10 / LW-11 | 3.998 × 10⁻⁸ | — | Derived |
| LW-52p | Habitable-zone worlds per star, LW-3 × LW-5 + LW-4 × LW-6 | 0.200 | — | Derived (LW-4 unverified as an empirical value) |
| LW-53p | Luck for 5 expected living worlds within 81.5 ly, perfect detection | 5.10 | dex | Derived, §2.3 |
| LW-54p | The same within 1,000 ly | 1.97 | dex | Derived, §2.3 |
| LW-55p | Prior probability that fLife × fMinds × fTech × L ≥ 3.2 × 10⁷ | about 2.9 × 10⁻⁴ | — | Estimate (Monte Carlo, 4 × 10⁶ draws, and a closed-form approximation). Needs an exact package computation |
| LW-56p | Sandberg: prior probability N < 1 (alone in the Galaxy) | 0.52 | — | Sandberg et al. 2018 §3.2. Verified (text) |
| LW-57p | Sandberg: the same after the Fermi observation, low and high | 0.53 and 0.996 | — | Sandberg §4. Verified (text) |
| LW-58p | Sandberg: prior probability N < 10⁻¹⁰ (alone in the observable universe) | 0.38 | — | Sandberg §3.2. Verified (text) |
| LW-59p | Sandberg: the same after the Fermi observation, low and high | 0.39 and 0.85 | — | Sandberg §4. Verified (text) |
| LW-60p | Sandberg: median and mean of N under Table 1 | 0.32 and 2.7 × 10⁷ | civilisations | Sandberg §3.2. Verified (text) |
| LW-61p | Fraction of the eight-dimensional haystack searched | 6.0 × 10⁻¹⁸ | — | Wright, Kanodia and Lubar 2018, §3 of the arXiv version. Verified (text). Check against the published AJ value |
| LW-62p | The same as seawater | 8000 | litres | Same; ocean volume 1.335 × 10²¹ l |
| LW-63p | Breakthrough Listen 2017: stars searched; upper limit on the share within 50 pc with a continuous 10¹³ W transmitter | 692; 0.001 | stars; — | Enriquez et al. 2017. Verified (abstract) |
| LW-64p | Breakthrough Listen 2020: stars; power limits, Green Bank and Parkes | 1327; 2 × 10¹² and 9 × 10¹² | stars; W | Price et al. 2020. Verified (abstract) |
| LW-65p | Optical laser search: stars; share of warm Earth-size planets excluded | 5600; 0.001 | stars; — | Tellis and Marcy 2017. Verified (abstract) |
| LW-66p | Atmospheric lifetime of fluorinated technosignature gases, low and high | 2000 and 50000 | yr | Schwieterman et al. 2024, citing Mühle et al. 2010. Verified (text) in Schwieterman; Mühle not opened |
| LW-67p | Age beyond which direct evidence of a civilisation on Earth is unlikely | 4 × 10⁶ | yr | Schmidt and Frank 2019 §1. Verified (text) |
| LW-68p | Urbanised share of Earth's surface, upper bound | 0.01 | — | Schmidt and Frank §1, citing Schneider et al. 2009. Verified (text) in Schmidt and Frank |
| LW-69p | Half-lives of plutonium-244 and curium-247 | 8.08 × 10⁷ and 1.5 × 10⁷ | yr | Schmidt and Frank §2. Verified (text) there; check against a nuclear data table |
| LW-70p | Galactic crossing time at 0.1c | 650000 | yr | Hart 1975. Verified (text) from a reprint; not checked against the journal |
| LW-71p | Lifetime above which a colonisation front reaches Earth: with growth and saturation; without discretisation; zero growth | 2.6 × 10⁶; 2 × 10⁷; 1.3 × 10¹⁰ | yr | Newman and Sagan 1981. Verified (abstract record) |
| LW-72p | Upper limit on the galactic crossing time from stellar motions; the stellar speed assumed | 3 × 10⁸; 30 | yr; km/s | Carroll-Nellenback et al. 2019 §4.1. Verified (text) |
| LW-73p | Example probe range and speed | 10; 0.01 | ly; c | Carroll-Nellenback et al. 2019, Fig. 9 caption. Verified (text) |
| LW-74p | Mean age of detected civilisations over the population mean | 2 | — | Kipping, Frank and Scharf 2020. Verified (abstract); the distribution it assumes must be read from the body |
| LW-75p | Share of Earth-like planets older than Earth; mean head start | 0.75; 1.8 × 10⁹ | —; yr | Lineweaver 2001. Verified (abstract) |
| LW-76p | Galactic Habitable Zone, inner and outer radius | 7 and 9 | kpc | Lineweaver, Fenner and Gibson 2004. Verified (abstract). Gowanlock et al. 2011 disagree |
| LW-77p | Chance of a lethal gamma-ray burst at Earth in 500 million yr; within 4 kpc of the centre | 0.5; 0.95 | — | Piran and Jimenez 2014. Verified (abstract) |
| LW-78p | Share of local disc galaxies with galaxy-wide star-shrouding engineering, conservative and tentative limits | 0.03 and 0.003 | — | Zackrisson et al. 2015. Verified (abstract) |
| LW-79p | Dyson-sphere candidates per objects searched; of which shown contaminated | 7 per 5 × 10⁶; 3 | — | Suazo et al. 2024; Ren et al. 2024. Verified (abstracts) |
| LW-80p | Galaxies searched for waste heat; number above 85 % reprocessing | 10⁵; 0 | — | Griffith et al. 2015. Verified (abstract) |
| LW-81p | Multiplicity of solar-type stars: single, double, triple, higher | 0.56, 0.33, 0.08, 0.03 | — | Raghavan et al. 2010. Verified (abstract) |
| LW-82p | Free-floating or wide-orbit planets per star, 0.33 to 6,660 Earth masses | 21 | per star | Sumi et al. 2023. Verified (abstract); its uncertainty must be read from the body |
| LW-83p | Radar beam as a fraction of the sky, for EIRP 2 × 10¹³ W and a 1 MW transmitter | 5 × 10⁻⁸ | — | Derived from OB-30. The 1 MW transmitter power is **unverified** |

Rows that an implementation of rank 2 or rank 3 would also need, and for which **no source exists**: the prevalence of each technosignature class among living worlds; the share of ended civilisations that leave a long-lived remote signature; the recurrence rate of technological lineages at zero dial. These would be Design rows and must say so.

### 5.5 Flags against "the anthropic dial never makes a quiet civilisation loud"

| # | Where | The breach, if adopted as written | The way to stay inside the rule |
|---|---|---|---|
| 1 | §3.1 | Loudness class depends on age; the dial moves lifetime; lifetime moves age | Keep loudness independent of age, or rule on it |
| 2 | §3.2 | Megastructure prevalence tied to lifetime raises loud remnants when lifetime is dialled | Keep the prevalence independent of the dialled lifetime |
| 3 | §3.3 | Settlement multiplies occupied worlds, and their signs, as lifetime rises; it is a fifth factor in the count | Leave it out of the default; if added, name its share in the luck statement |
| 4 | §3.4, rank 2 | Not a loudness breach, but a change to D-144's wording: the dial would act on a recurrence rate instead of fractions that saturate at 1 | A ruling that "minds" and "technology" may be dialled as rates |

Rank 1 and rank 5 touch neither rule. Rank 3 stays inside the rule provided flags 1 and 2 are observed. Rank 4 stays inside it.

### 5.6 Rulings asked for

1. Should the calibration count encounters over the mission range (1,000 ly as the reference) instead of the 81.5 ly map? (§3.0, rank 1.)
2. May the dial act on a recurrence rate, so that a planet can host successive technological lineages? If so, which row of the §3.4 table is the default universe: ancients (3.2 × 10⁷ yr, today's), a middle course (10⁶ yr), or mortals (10⁵ yr)? (Rank 2, flag 4.)
3. Is loudness independent of age? (Flag 1.)
4. Is settlement out of the default? (Rank 6, flag 3.)
5. The adjusted targets of §5.3: adopt, amend or keep the current set.
6. Which statement of luck leads in the Archive: the factor over the median, or the 1-in-N percentile? (§3.8.)

### 5.7 What I am unsure of

- **The calibration's internals.** §2.3 reproduces its headline (5.1 dex and 25 million years against 10⁵ and 32 million), which suggests the reconstruction is right, but I have not seen its output. Every "luck saved" figure is relative to my reconstruction.
- **The radar beam.** The 5 × 10⁻⁸ assumes a 1 MW transmitter, from memory. If the calibration already charges a beam factor, §3.1's third calculation is wrong about what the 5–10 signals rest on.
- **Whether the package excludes binaries.** I did not find it stated.
- **The prior percentile.** 2.9 × 10⁻⁴ comes from a throwaway Monte Carlo and a closed-form approximation that agree. It treats fLife as "at least 0.99" for X above 2 in the rate model. It should be computed exactly in the package. The "1 in 15 to 1 in 20" figure for 2.5 to 3 dex is from the same kind of run. The percentile falls slowly at first because the life factor alone is near 1 in about half of all draws.
- **The time structure.** ±0.5 dex is a judgement from four abstracts, not a calculation.
- **Abstract-only sources.** Most sources were opened at the abstract only (see §6). Numbers inside the body of those papers are not checked. In particular: Kipping et al.'s factor of two depends on a distribution I did not read; Grimaldi and Marcy's numbers were garbled in the fetch and are stated loosely; the Benford beacon paper is cited for its subject only.
- **The remaining habitable time for Earth** (about a billion years) is from memory and is used only as context.

---

## 6. Source ledger

**Opened in full text and read at the passage cited (Verified, text):** Sandberg, Drexler and Ord 2018; Wright, Kanodia and Lubar 2018 (arXiv version); Carroll-Nellenback et al. 2019; Schmidt and Frank 2019; Socas-Navarro et al. 2021; Schwieterman et al. 2024 (the lifetime passage); Carter 1983 (first page and abstract); Hart 1975 (a reprint with OCR errors, not checked against the journal); the Science News article quoting Drake (2010).

**Opened at the abstract only (Verified, abstract):** NASA Technosignatures Workshop 2018; Benford 2019; Benford, Benford and Benford 2010; Wright et al. 2014 (Ĝ I); Griffith et al. 2015; Suazo et al. 2024; Ren, Garrett and Siemion 2024; Zackrisson et al. 2015; Hanson et al. 2021; Lingam and Loeb 2017; Lingam and Loeb 2019; Adams and Spergel 2005; Kipping, Frank and Scharf 2020; Kipping 2020; Balbi and Ćirković 2021; Grimaldi 2017; Grimaldi, Marcy, Tellis and Drake 2018; Grimaldi and Marcy 2018; Lineweaver 2001; Lineweaver, Fenner and Gibson 2004; Gowanlock, Patton and McConnell 2011; Behroozi and Peeples 2015; Mor et al. 2019; Piran and Jimenez 2014; Snyder-Beattie et al. 2021 (via Europe PMC); Mills et al. 2025; Westby and Conselice 2020; Frank and Sullivan 2016; Wright 2018; Wright et al. 2022; Socas-Navarro 2018; Stevens, Forgan and O'Malley-James 2016; Haqq-Misra et al. 2022; Forgan and Nichol 2011; Saide et al. 2023; Sheikh et al. 2025; Enriquez et al. 2017; Price et al. 2020; Sheikh et al. 2020; Sheikh et al. 2021; Smith et al. 2021; Tellis and Marcy 2017; Méndez et al. 2024; Abbot and Switzer 2011; Sumi et al. 2023; Shields, Ballard and Johnson 2016; Raghavan et al. 2010; Gray 2015; Newman and Sagan 1981 (the NASA NTRS abstract record).

**Could not open (Unverified):** Tipler 1980 (known from secondary summaries only); Tarter et al. 2010 (its claim is confirmed only as described by Wright, Kanodia and Lubar); Dyson 1960; Schwartz and Townes 1961; the original Wow! signal report; Mühle et al. 2010 (cited through Schwieterman et al.); Sullivan, Brown and Wetherill 1978 (the observation registry records the same gap); the Arecibo transmitter power.

**Already verified elsewhere in this repository and relied on here:** Wright et al. 2014 (Ĝ II) and the radar and television budgets, in [physics/observation.md](../../physics/observation.md); the baseline factors, in [physics/life-and-worlds.md](../../physics/life-and-worlds.md).

No citation in this document is included without one of these labels.
