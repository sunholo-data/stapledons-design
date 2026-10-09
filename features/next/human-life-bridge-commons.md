# Human life aboard: bridge and Level 1 Commons

**Status:** Design interview in progress, 2026-10-09. Scope and priorities below
are Mark's attended instructions; proposed mechanics are not approved for execution.
**Release:** R2 candidate; scheduling remains open. No expansion of the active R1 sprint.
**Next deliverable:** the [shared social dynamics and generated-event design](shared-social-dynamics-and-events.md), then a concrete assignments/projects/resource slice for this area,
followed by a Godot implementation design and an approved sprint.

## Game vision alignment

| Pillar | Score | Reason |
|---|---:|---|
| Choices Are Final | +2 | Projects and promises leave persistent consequences. |
| The Game Doesn't Judge | +2 | Competing needs have consequences rather than moral scores. |
| Time Has Emotional Weight | +2 | Work and relationships develop on ship time; contact changes on external time. |
| The Ship Is Home | +2 | The existing bridge and Commons become inhabited, useful places. |
| Grounded Strangeness | +1 | Contact works through signals across a sealed boundary. |
| We Are Not Built For This | +2 | Competence, care, fatigue and disagreement constrain captain authority. |

Net +11: aligned direction. This is a design assessment, not an execution gate.

## Recorded scope and priorities

Mark, attended 2026-10-09:

- "we have many bubble ship levels to design but we will stick to the bridge
  and first level for now"; these form the background for human interactions.
- The first level is already designed: find and use the references.
- Quiet transit priority: "Shape ship life through assignments, projects and
  resource decisions".
- First contact priority: "Build a lasting relationship through negotiation,
  promises and repeated visits".

- Captain trust and authority, crew-to-crew reactions and task-driven indicators underpin
  an emergent sandbox. AI creates content/events from internal conditions, which
  influence the bubble's external actions and civilisation responses. The same causal
  framework operates at micro and macro scales; see [shared dynamics](shared-social-dynamics-and-events.md).

- There is no "right" way to play: different priorities produce different trajectories.
  Indicators describe consequences rather than a prescribed ideal society or captain.

These statements do not select project costs, crew authority/refusal rules,
contact frequency, alien biology or an implementation schedule.

## Existing spatial references — do not redesign the Commons

The first level below the bridge is **Commons**, floor Z +57 m. The bridge is
Z +82 m in the approved seven-tier study. Dimensional authority remains the
measured asset assembly, not an illustrative concept.

References in the sibling game repo (`../../..` from this file reaches the
design repo's parent; these links assume the normal sibling checkout layout):

- [Approved Commons architecture and continuation](../../../stapledons-godot/design_docs/planned/r1/m4-ship-commons-build.md):
  sweeping terraces, open curved arcades, planted promenades, civic plazas,
  warm inhabited spaces, seating and an Archive approach beside the spire.
  The 2026-10-04 review correction rejects the enclosed barrel-roof pavilion.
  The later open-arcade continuation approves the measured zoning direction
  and a bounded plaza/arcade sample, not completion of the entire tier.
- [Approved Level 1 concept](../../../stapledons-godot/art/ship-tier-concepts-v1/commons.png)
  and [concept provenance](../../../stapledons-godot/art/ship-tier-concepts-v1/README.md).
  Art direction, not measured geometry or a painted physical sky.
- [Commons zoning SVG](../../../stapledons-godot/art/ship-commons-v1/style-preview/commons-plan-proposal-v1.svg).
  Its older README calls it a proposal; the later architecture continuation
  records approval. That later dated approval wins.
- [Unified painted ship](../../../stapledons-godot/design_docs/planned/r1/m4-unified-painted-ship.md):
  textured 3D geometry and sky through one observer, with physical occlusion.
- [Ship UI and consoles](../../../stapledons-godot/design_docs/planned/r1/ship-ui-hud-consoles.md):
  information in the HUD, consequential ship decisions at bridge consoles;
  reserved crew-decision consoles are an integration seam, not permission to edit them.

The rest of the ship remains context/background for this first design. Do not
add playable quarters, medical rooms, gardens or workshops on other levels.
Tasks elsewhere may be represented by reports if later approved, without
claiming those locations are built or traversable.

## Design foundations and precedence

- [Premise and loop](../../vision/premise-and-loop.md): current canon, autonomous
  ship society, Earth mission, recursion, cross-run content without surviving memories.
- [Bubble constraint](../future/bubble-constraint.md): no mass crosses either way.
  No landing, visitors, cargo exchange, artifact pickup, or probes launched from aboard.
- [Crew assignments](crew-assignment-system.md), [bubble society](../future/bubble-society.md),
  [crew psychology](../future/crew-psychology.md) and [cast](../../art/generation-cast-plan.md):
  existing direction; legacy bonuses, schedules and proposed family details are not approvals.
- [AI showcase](../ai-showcase.md): independent AI service, recorded output and cached
  expression. A model cannot silently invent resources, consent or world-history changes.
- [Civilisation trade](../future/civilization-trade.md),
  [world evolution](../future/planet-state-transitions.md) and
  [life calibration](life-and-intelligence-parameters.md): contact foundations;
  generation rates and encounter targets still need decisions and calibration.

The game ledger D-34/D-35/D-52/D-56/D-57 governs current geometry, presentation
and consoles. Old fixed scenes, independently moving panorama plates, twenty-deck
geometry and decision shortcuts are historical references.

## Proposed next design: a ship project with human consequences

Develop one bounded project during transit. The player hears needs and competing
proposals in the Commons, assigns responsibility and resources through the existing
decision framework, observes progress, and experiences the result through people
and the shared space. A later contact promise can create competing demands.

The design must specify:

1. Who proposes work, who can accept/refuse it, and how captain authority works.
2. What is scarce: available work time, skills, recoverable materials, power,
   fabrication capacity or something else; avoid interchangeable abstract budgets.
3. How work competes with essential duties and people's own lives.
4. What takes ship time, what can change, and what consequences cannot be undone.
5. How progress appears in the existing Commons without replacing approved architecture.
6. What characters remember and how a promise becomes an actionable commitment.
7. How negotiation creates mutual obligations without controlling alien choices.

No particular project, resource value or relationship outcome is selected yet.

## Alien generation and contact: next design boundary

Proposed contract to develop after the ship loop: seeded environments, biospheres,
species and societies with independent histories, distinct from what the crew knows.
Keep biology, culture, government and a particular speaker separate. Avoid treating
a whole civilisation as one personality or one technology level.

Generate expression from constrained facts; validate and record it. Translation
quality and differing timescales affect negotiation. Promises must record parties,
terms, conditions and dates; long journeys may outlive the people or institutions
that made them. Revisits reveal how others used information, not guaranteed rewards.
These are proposals for review, not newly ratified canon.

## Interview still needed

- Which resources and kinds of project should create the first meaningful conflict?
- Is an assignment an order, a negotiated commitment, or dependent on the person/task?
- What happens when somebody refuses, fails, or needs relief from essential work?
- How do quiet transit and accelerated travel leave room for ship life?
- What can the captain promise an alien society that matters aboard the bubble?
- What obligations survive changed governments, generations and missed return dates?

## Completion criteria for the design pass

- Every existing spatial/character reference is identified before proposing replacements.
- Facts, attended decisions, proposals and unanswered questions remain distinct.
- One concrete project example identifies actions, costs, progress and consequences.
- One contact/revisit example identifies mutual agency, signals and elapsed time.
- The implementation design names command-checkable acceptance criteria and preserves
  deterministic replay, closed mass, signal causality and generator/judge independence.
- No active renderer, navigation, UI, assets, mission schedule or package is changed.

## Implementation/interview update, 2026-10-09

The captain-and-crew CLI lab now provides readable state for rapid interior-policy
iteration without building the game; see the [shared dynamics update](shared-social-dynamics-and-events.md#captain-and-crew-cli-iteration-attended-2026-10-09).
Bridge and first-level layout references above remain the background for eventual
presentation. Mark selected crew reacting to each other and spreading disagreements
as the next experiment. This does not mark the bridge/Commons interactions as
integrated into the playable dev app.

Mark approved a growing AI-authored local response library and OCEAN-sensitive
automatic crew reactions. The next CLI slice presents numbered captain choices,
then crew dialogue and consequences; it precedes peer disagreement spreading.
See the [response-library direction](shared-social-dynamics-and-events.md#growing-response-library-and-personality-attended-2026-10-09). Bridge and Commons
remain the first game presentation scope; no new room geometry is decided here.
