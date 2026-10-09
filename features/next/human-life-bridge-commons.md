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
automatic crew reactions. The standalone CLI now presents numbered captain choices,
automatic crew dialogue and exact gauges, with banked GLM5.3Flash content, explicit
seeds and replay journals. Installed offline flows and live miss/cache repeat pass;
PR110 CI green, final independent evaluation100/100; merged to packages main. It precedes peer disagreement spreading.
See the [response-library direction](shared-social-dynamics-and-events.md#growing-response-library-and-personality-attended-2026-10-09). Bridge and Commons
remain the first game presentation scope; no new room geometry is decided here.

## Standalone captain game presentation, attended 2026-10-09

Mark clarified that the CLI should be a small game in its own right: assume a new
player does not know the simulation or its commands. The follow-up now adds
a bridge briefing, readable crew/supply/project panels, contextual help, explanations
of consequences and a factual session recap. Ordinary resource or consent rejection
must keep the captain playing. Existing bridge and Commons references remain the
setting; no room geometry, social rules or resource costs change in this slice.

Each launch starts a fresh crew session; the local response library carries over.
There is no victory grade or single correct route. The player learns by comparing
how assignments, rest, authority and limited supplies change the ship's people.
This usability slice precedes crew-to-crew disagreement spreading. Implementation
is in [packages PR112](https://github.com/sunholo-data/ailang-packages/pull/112):
merged to packages main. Final CI37958354490 and independent review100/100 pass;
109 named app controls and 14 library controls pass on each engine. The installed
command was verified with the existing library and zero fresh AI calls. The design
and evidence are archived with the package sprint. No provider calls were needed
to develop this update.

## Journey workshop UI correction, attended 2026-10-09

Mark tried that presentation and judged it still unclear: the terminal needs
serious UI work and enough context for a newcomer. Passing host controls and
the earlier independent implementation review did not establish that the lab
was an understandable game. The current installed player remains a scrolling
numbered prototype while the replacement is developed.

Mark selected **a strained crew preparing for first contact** as the first
journey example and explicitly prefers **pure AILANG for the terminal UI**.
The implementation direction is a reusable terminal UI package and a composed
captain screen with Bridge, Crew, Work, Log and Guide views. Reading details
must preserve the pending decision, time, random seed and AI allowance. Crew
conversation and consequences lead; debug evidence and detailed meters are
inspectable. Current runtime input remains keys followed by Enter; native keys
and terminal-size events have been requested from AILANG core.

The first interface slice supplies a preparation chapter using existing rules.
It does not yet simulate alien negotiations or whole multi-leg journeys. The
workshop continuation is scenario authoring, condition-driven crew incidents,
peer evidence/disagreement, promises, contact/revisits and a causal journey
chronology. Those additions need concrete mechanics and acceptance tests,
without assigning a single correct way to play or changing Commons geometry.
