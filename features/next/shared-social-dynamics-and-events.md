# Shared social dynamics and generated events

**Status:** Design draft and interview, 2026-10-09. Mark's direction is recorded
below; indicator choices, numerical rules and event contracts remain proposals.
**Implementation direction (Mark, attended):** reusable AILANG package support,
with standalone experiments that need no game build. Experimental package
`sunholo/social_dynamics` and [implementation design](/Users/voightkampff/dev/sunholo-data/stapledons-godot/.ailang/cache/reviews/social-dynamics-20261009/packages/design_docs/implemented/0.1.0/social-dynamics-0.1.0.md);
[sprint](/Users/voightkampff/dev/sunholo-data/stapledons-godot/.ailang/cache/reviews/social-dynamics-20261009/packages/design_docs/implemented/0.1.0/social-dynamics-0.1.0-sprint.md) implements Mark’s approved sprint; native, replay and strict-VM checks pass. Independent final evaluation is recorded with the package.

**Scope:** Shared micro/macro framework, first exercised on the bridge and
Commons. The standalone pure kernel is implemented; ship/UI/navigation integration remains separate.
**Related:** [human-life scope](human-life-bridge-commons.md),
[society](../future/bubble-society.md), [psychology](../future/crew-psychology.md),
[Archive trust](../future/archive-crew-trust.md),
[orchestrator](../future/narrative-orchestrator.md),
[contact](../future/civilization-trade.md), [AI service](../ai-showcase.md).

## Game vision alignment

| Pillar | Score | Reason |
|---|---:|---|
| Choices Are Final | +2 | Decisions alter a persistent causal history. |
| The Game Doesn't Judge | +2 | Indicators describe conditions, not goodness or victory points. |
| Time Has Emotional Weight | +2 | Fatigue, promises and external responses evolve on their own clocks. |
| The Ship Is Home | +2 | Crew react to each other and the captain, shaping ship capability. |
| Grounded Strangeness | +1 | Cultures have distinct models and encounter constraints. |
| We Are Not Built For This | +2 | Maintaining workable authority requires people, capacity and care. |

Net +11: direction aligned; no approval of the numerical model implied.

## Attended direction

Mark describes captain trust and authority as part of making the mission succeed.
Crew react to the captain and each other. Tasks contribute to underlying indicators
(possibly KPIs), whose excessive or inadequate values cause consequences and events.
OCEAN and other personality/relationship state shape reactions. A similar framework
operates for player-mediated interactions across the universe, at micro and macro scales.

The intended experience is **sandbox emergence**: dynamic AI content and events
respond to internal environmental conditions. Those conditions influence how the
bubble ship acts externally, and therefore how civilisations respond and develop.
This is not merely a dialogue layer over predetermined event scripts.

## Different ways to play — no prescribed right path

**Mark, attended 2026-10-09:** there is no "right" way to play, but different ways.

Indicators describe the society the player is creating, not a universal target to
maximise. A mission-focused captain, a community builder, an explorer or a cautious
negotiator may create different viable trajectories. These are examples, not classes,
scored identities or an exhaustive menu. Players can change priorities over time.

There is no combined goodness score, ideal KPI vector or reward for balancing every
indicator. Thresholds describe particular material or social consequences: they do not
rank a life, philosophy or ending. Different people and institutions may value the same
outcome differently. The captain can accept costs deliberately, and some commitments
conflict without an option that satisfies everyone.

Maintaining authority enables particular plans; it is not the sole measure of a
worthwhile run. Saving Earth remains a difficult possible goal, not the definition of
correct play. Failure, refusal, lost authority and mutiny can remain real consequences
without a moral verdict. No-right-path does not mean consequence-free play or that
all strategies are equally survivable.

Design and evaluation should explore contrasting strategies and demonstrate distinct,
causally understandable trajectories. Check for accidental dominant strategies and
one-dimensional optimisation; do not require equal outcomes or guaranteed success.
Generated events should recognise chosen priorities without steering the player back
towards a designer-preferred balance or manufacturing punishment for unconventional play.

## Proposed causal loop

Conditions → task or situation → captain/crew decisions → work and consequences
→ changed conditions and relationships → new generated situations.

External branch: internal priorities/capacity → signals, knowledge, promises or
withholding → recipients' decisions → local and network consequences → later signals
or contact → new pressures aboard.

Do not collapse this into one universal score. Reuse the causal machinery, while
different actors and societies have different needs, interpretations and agency.

## Indicators, relationships and authority

Candidate dimensions for the first ship slice:

| State | Scope | Too little / too much: candidate consequences |
|---|---|---|
| Operational readiness | Equipment/team | Failures and deferred work / costly overinvestment at the expense of other needs |
| Workload and recovery | Individual/team | Unmet essential work / fatigue, errors and strain |
| Material availability and committed resources | Physical inventory/project | Inability to start work / stockpiling with opportunity costs, not arbitrary punishment |
| Social cohesion and unresolved conflict | Group, retaining individual differences | Failure to cooperate / conformity only if separate evidence shows dissent is suppressed |
| Mission commitment | Person/faction | Departure from shared purpose / overcommitment that ignores health or other values |
| Reliability of promises | Directed relationship and commitment history | Conditional cooperation or distrust / high reliability is beneficial, but new obligations still cost capacity |

These are candidates, not selected KPIs. Physical quantities retain units; social
indices require documented meaning, bounds and calibration. Each task names its
costs, affected actors, progress conditions, immediate effects and delayed effects.
Do not equate every high value with a problem. Some dimensions have a desired range,
some are beneficial when high, and some measure pressure or risk. Distinguish state
levels from trends, duration and uncertainty.

Trust is **directed**: a person can trust the captain's competence while doubting
their honesty or care; trust in a colleague or the Archive can differ. Propose
separating competence, reliability and perceived care before choosing the minimum model.

Authority is the practical ability and accepted right to secure cooperation. Formal
office, legitimacy, confidence in the mission and faction backing contribute; a
successful repair is not automatically a universal authority bonus. The exact
relationship to orders, bargaining, refusal and mutiny remains for the interview.

OCEAN modifies human appraisal alongside needs, values, experience, relationships,
knowledge and current strain. It does not make an archetype determine a response.
Do not copy human OCEAN onto alien species by default or give a civilisation one
personality; model institutions, groups and speakers separately.

## Consequences and event eligibility

Propose thresholds with duration, recovery bands and cooldowns so a condition does
not repeatedly fire on every tick or oscillate around a boundary. Trends and combinations
matter: fatigue plus a broken promise plus distrust can produce a different situation
from any of them alone. Include opportunities, celebrations and mundane adaptations,
as well as conflict and failure.

Physical failure can have direct consequences independent of narrative selection.
Social responses depend on who experiences or learns of an event. Never apply
omniscient trust changes to everyone. A delayed or secret event may propagate later.

Critical events need visible precursors and options appropriate to actual authority.
No reflex timer is introduced. Low cooperation can constrain a mission without being
a moral judgement; high tension is not an automatic loss. Mutiny still requires the
existing canon and a separately specified decision process.

## Dynamic AI event creation — proposed contract

The AI may invent concrete situations, requests, projects, disagreements and contact
proposals from current conditions, rather than only select text from a fixed pool.
It receives actor-specific knowledge, relevant relationships, location, commitments,
resources, recent causal history, cultural constraints and available action types.

A generated proposal contains:

- Participants, location, cause and relevant history references.
- Preconditions, actor knowledge and timing on an explicitly named clock.
- Situation, participant goals and possible actions; captain options need not exhaust
  what other actors can do.
- Costs, effects, bounds, progress rules and follow-up eligibility expressed through
  supported simulation operations, with links to affected commitments.
- Dialogue/visual requests and provenance separate from mechanical effects.

The simulation validates IDs, causality, affordability, authority, location, closed
mass and supported operations before accepting a proposal as a recorded input.
AI cannot run arbitrary effect code, change seeded history retroactively, grant
free resources, silently change personality, or prescribe a civilisation's response.
New categories of mechanics require deliberate implementation; new situations within
the supported grammar can be generated freely.

Record accepted proposals, effects and expression so replay needs no AI calls.
Record rejection reasons for debugging and use bounded retries/fallbacks without
blocking the simulation. Core no-key content must exercise the same mechanics;
live AI stays opt-in under the existing operating model. Pacing selects among
eligible situations; it cannot fabricate a shortage solely to force dramatic tension.

## Illustrative micro-to-macro chain (not selected first content)

An alien community requests regular observations for a long-term environmental study.
The Scientist supports a shared project; the Medic reports an exhausted observation
team. The captain commits to a schedule and assigns work without relieving other duties.

Initially, research progresses and the alien collaborators rely on the data. Fatigue
and delayed ship maintenance then accumulate. One crew member asks to renegotiate;
another sees withdrawal as betrayal. Depending on choices, the crew redistribute work,
reduce the promised scope, miss a transmission or continue at personal cost.

The other society reacts through its own participants and institutions: it may adapt,
seek another partner, dispute the promise or maintain cooperation. News reaches the
bubble at the appropriate signal delay. A later visit reveals the relationship and
project that actually developed, not a predetermined reward or punishment.

Other civilisations can learn of the collaboration through their own connections.
Effects propagate through particular actions and signals, not a galaxy-wide instant
reputation delta. An unvisited society continues its own history.

## Design verification to define before implementation

- Same initial state and recorded inputs reproduce indicators, events and effects.
- Tick size and warp do not change task cost, event counts or elapsed-time consequences.
- Ship routines use proper time; world histories use external time; messages respect causality.
- Relationships remain directed and observers differ from uninformed actors.
- Threshold entry/recovery/cooldown behaviour avoids event spam and hidden instant loss.
- Generated invalid costs, actors or effects are rejected without corrupting state.
- A no-key run and an accepted generated event both exercise the shared mechanics.
- A contact promise creates traceable ship costs and recipient choices, with no guaranteed ending.

## Interview decisions outstanding

1. Indicator visibility: numbers, qualitative reports with detail, or behaviour only?
2. Authority loss: graduated resistance, faction bargaining, or commands with accumulating resentment?
3. Initial dimensions and project: what competing needs make the first slice meaningful?
4. Which actor actions may AI generate autonomously, and which require a captain decision?
5. Transit pacing: how do routine work and conversations fit accelerated journeys?
6. Alien obligations: what terms survive time gaps, institutional change and lost communication?

## First experiment implementation, attended 2026-10-09

Mark approved the reusable kernel sprint (“yep approved”). Experimental `sunholo/social_dynamics@0.1.0` now implements bounded indicators, directed evidence-gated reactions, explicit task assignment/consent, conserved project materials, hysteresis conditions, one-clock scheduling and atomic generated proposals. Three ship paths and two community paths run standalone without a game build or live AI. Local publication remains unauthorised.

[Runner guide](/Users/voightkampff/dev/sunholo-data/stapledons-godot/.ailang/cache/reviews/social-dynamics-20261009/packages/examples/social-dynamics/README.md) and [validation](/Users/voightkampff/dev/sunholo-data/stapledons-godot/.ailang/cache/reviews/social-dynamics-20261009/packages/design_docs/implemented/0.1.0/social-dynamics-validation.md). Numerical example choices remain tuning fixtures rather than game canon. Bridge/Commons presentation, captain KPI visibility, final refusal/authority rules, live generated proposal adapter and alien life/civilisation models remain open. Actual task/condition outcomes become immutable evidence; crew/counterparts react only after host-received delivery.

## Captain-and-crew CLI iteration, attended 2026-10-09

A second standalone `sunholo/crew_lab@0.1.0` experiment combines the social kernel
with `sunholo/decisions@0.4.0` saved synthetic choices. It binds each request to
received actor context, its exact project/agreement, ship tick and revision; banks
alternatives and explicit sampling rolls; and compares experimental orders/consent
policies, relief, fatigue, work, materials and directed trust. `crew-lab` keeps the
full causal JSON; `crew-view` presents readable state after each captain step.
A complete six-command run measured about1.4s locally, without a Godot build.

[CLI guide](https://github.com/sunholo-data/ailang-packages/tree/main/examples/crew-lab)
and [reviewed implementation](https://github.com/sunholo-data/ailang-packages/pull/108).
Independent evaluation100/100:56 native controls on both interpreter and strictVM,
five complete replay/parity paths and eight compiling mutants killed. This is an
offline tuning fixture: traits enter context, fixed trust deltas illustrate received
reactions, and synthetic choices do not prove a model used OCEAN. The later guided
CLI slice below adds trait-weighted reactions and live/cached dialogue; bridge/Commons
UI integration remains future work.

**Mark's selected next CLI experiment:** “Crew reacting to each other and spreading
disagreements.” Explore disagreements received through conversation and shared
projects, diverging worker responses and captain mediation; preserve explicit
knowledge and causal receipts. Selection sets the next design direction, not final
numeric tuning or universal gossip/authority rules.

## Growing response library and personality, attended 2026-10-09

Mark approved automatic crew replies with a numbered captain interface: captain
chooses assignments, priorities and responses to concerns; crew choose personal
reactions. AI creates dialogue for a missing situation, and validated responses
accumulate in a local library across playthroughs. Reusing a library does not mean
reusing the last selected decision: response probabilities and text variants can
be sampled afresh. A recorded run retains its exact choices for replay.

OCEAN is the human personality scaffold, alongside values, fatigue, directed
relationships and known events. Profiles influence probabilities and dialogue
tone, rather than assigning an archetype one inevitable action. Existing lab
profiles already enter perception; the first fixtures did not demonstrate
automatic trait-dependent reactions. The new experimental policy makes each
trait's influence testable. Its coefficients are tuning choices, not scientific
calibration or a moral score. Personality drift remains future work.

The reusable library persists suitable content, while promises, grievances,
discoveries and relationships stay in the current universe. Cache suitability
must include personality and relevant received context; characters cannot learn
from unseen events. Initial matching is deliberately exact and conservative;
situation bands and broader retrieval need separate behavioural evidence.
Generation supplies text variants; host-validated actions determine consequences.
Narrative structural checks cannot establish that every generated sentence is true.

Mark initially chose Gemini Lite; Google rejected the existing laptop key as
API_KEY_INVALID. He then selected the existing OpenRouter key with GLM5.3Flash
(z-ai/glm-5.3-flash). An attended real CLI encounter generated a valid bundle in one
call (1058input/2692output tokens); an identical encounter reused the same selection
and seed progression from cache with zero calls/current tokens. New content is
generated on exact-context misses, with a bounded attempt budget; cache hits make
no new provider call. A labelled authored offline path remains available.

The implemented CLI slice provides numbered captain menus, portable exact gauges,
trait-sensitive response policy, generated/cache dialogue and a complete run
journal. Local checks pass:94app+14library native controls on each engine, four
installed offline flows with complete host replay, cache/error/journal failure
controls and eight behavioural mutants. Independent final review100/100 and PR110 CI green; merged to packages main.
The selected
crew-to-crew disagreement-spreading experiment follows this playable foundation.
Implementation: [packages PR110](https://github.com/sunholo-data/ailang-packages/pull/110),
with sunholo/content_library@0.1.0 and crew-play/crew-play-offline. This is a
standalone CLI feature; Godot bridge/Commons integration remains separate.

### New-player presentation and recovery

The approved CLI slice in [packages PR112](https://github.com/sunholo-data/ailang-packages/pull/112)
introduces a standalone captain-game briefing over this foundation. Introduce
the role and situation before showing commands; explain
offer/agreement/start/time as separate steps. Display directed personal trust,
actual project progress and reserve-versus-consume accounting in terminal panels.
Help and navigation carry no social effects or random draws. A recorded rejection
explains its cause and returns to play; a journal publication failure still stops
before exposing unrecorded state or dialogue.

The existing fixture starts with six materials. Science reserves and consumes four;
maintenance needs three reserved and consumes two. Science completion therefore
does not replenish enough materials for maintenance. Explain this tradeoff honestly
without creating new resources or calling one play style correct. A run recap is
descriptive, and journals are records rather than a resumable save implementation.
Reviewing an existing offer is navigation, so it does not request another crew
answer or consume dialogue allowance. Saved stock narration is distinguished from
saved/new AI quotations, with the complete original selection and provenance kept
in the journal. Help and the latest crew response stay beside the next choice.
PR112 is merged to packages main; final CI37958354490 and independent review100/100
pass. The installed CLI reuses the existing dialogue library with no fresh AI call.
The completed design, companion sprint and independent evidence were archived in
[PR113](https://github.com/sunholo-data/ailang-packages/pull/113).

### Attended usability correction: a journey workshop

After playing that version Mark judged the numbered, scrolling presentation still
unclear. Its passing controls establish host behavior, not newcomer comprehension
or compelling stories. The replacement interface is a composed, paged **pure AILANG**
terminal UI, with crew conversation/current decisions in focus and Crew, Work,
Log and Guide views for inspection. Reusable `sunholo/terminal_ui` and the new
`crew-journey` / `crew-journey-offline` entries are merged to packages main and
installed from a durable isolated checkout. Registry publication remains separate.
Raw keys/resize have been requested upstream (canonical message
`inbox_1791568104057_d08245d1`); current input is a short key followed by Enter.

[PR114](https://github.com/sunholo-data/ailang-packages/pull/114) and its
[archive PR115](https://github.com/sunholo-data/ailang-packages/pull/115) are merged.
Final [CI37973618460](https://github.com/sunholo-data/ailang-packages/actions/runs/37973618460)
and independent98/100 acceptance pass. Both engines pass125crew/14library/12terminal
native controls; seven installed view-navigation flows preserve exact journal
payloads and replay; eight mutants and the existing social regression pass.
Actual 80x28/60x24 screens, cached long replies and safe widths were reviewed.
Installed science with the existing AI cache completes at turn4 with observations25,
fatigue70 and materials2 available/0 reserved/4 consumed, with zero fresh calls.
Raw proof limitations remain explicit: crew17/83 verified,64skipped,2encoder errors;
terminal2/9 verified,7skipped. No new social effects or provider/model change.

Mark selected a strained crew preparing for first contact as the first story
example. The interface's first chapter uses current work/rest/authority rules;
it does not claim alien negotiations or multi-leg journey simulation already
exists. The intended workshop supports authored scenarios, condition-triggered
incidents, peer evidence/reactions, promises and later contact/revisits, with a
causal chronology for comparing runs. Keep AI expression cached and sampled,
while trusted rules own effects. New scenario costs, KPI meanings and recorded
schemas require their own reviewed design.
