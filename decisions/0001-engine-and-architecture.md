# ADR 0001: Godot 4 renderer + AILANG simulation sidecar

**Status:** Accepted (spike validated 2026-09-27)
**Supersedes:** the Go/Ebiten + Tetra3D engine in `sunholo-data/stapledons_voyage` (last commit `930eca1`)
**Spike:** `sunholo-data/stapledons-godot`

## Decision

- **Rendering, UI, input and audio run in Godot 4** (4.7.2, Forward+ on Metal).
- **The game simulation is written in AILANG.** It runs as a long-lived child
  process started by Godot:
  `ailang run --quiet --bytecode --package-dir sim --caps IO sim/<entry>.ail`
- **The runtime is hybrid.** Pure code runs on the bytecode VM. Effectful
  built-ins (`readLine`, `println`, `flush` today) aren't compiled for the VM
  yet (Phase 2E), so `--bytecode` bridges each of those calls to the
  interpreter, one call at a time.
  - The pure simulation core (`sim/core.ail`) is checked to run entirely on the
    VM with `--strict-bytecode` (`make strict`).
  - The I/O shell (`sim/ship.ail`) relies on the bridge until Phase 2E lands.
  - **Rule: hot simulation code stays pure.** An effect inside the tick loop
    would push that loop through the interpreter.
- **Physics comes from the published package `sunholo/relativity`**, which is
  shared with other AILANG users. The game's simulation depends on a pinned
  registry version.
- **The two talk newline-delimited JSON over stdin/stdout.** Each tick Godot
  sends one line (the input) and AILANG replies with one line (state or
  changes). The world state lives inside AILANG.
- **The simulation ticks at a fixed 10–30 Hz.** Godot interpolates between
  ticks and owns everything per-frame.

## Why rebuild at all

The design docs survive the old implementation. The implementation itself did
not hold up.

- **The relativity visuals were wrong.**
  - *Aberration had the wrong sign in both code paths:* stars moved away from
    the direction of travel instead of crowding toward it.
  - *The black-hole shadow was drawn at r_s instead of about 2.6 r_s.*
  - *Doppler used the wrong angle.*
  - *Brightness was scaled in 8-bit colour.*
  - *Gravitational redshift was computed from screen position.*

  Details are in [physics/relativity-spec.md](../physics/relativity-spec.md).
  None of it was covered by tests.
- **Ebiten/Kage can't support accurate effects.** It has no cubemaps, no
  floating-point (HDR) render targets and no compute shaders.
- **The AILANG part was thin.** About 4,550 lines of AILANG produced draw
  commands, while about 31k lines of hand-written Go did the rendering and
  demos.
- **The main source of pain is gone as a path.** AILANG's compile-to-Go
  (emit-go) is frozen upstream, and most of the bugs Stapledon hit were in that
  code generator.

## Why Godot

What the design docs require, and how Godot covers it:

| Requirement (from the design docs) | Godot 4 |
|---|---|
| UI-heavy modes: dialogue, journey planner, civ detail, legacy report, logbook | Control nodes, RichTextLabel, themes |
| 2D/2.5D painted deck scenes with a live 3D starmap in the window regions | SubViewport composited into 2D |
| Accurate SR/GR visuals | HDR float targets, custom spatial/sky/full-screen shaders, cubemaps, compute (RenderingDevice), instanced stars |
| Automated checking without a person watching | Text scenes and scripts, headless runs, command-line rendering to PNG, golden tests |
| Desktop platforms; web possible later | Exports for macOS, Windows, Linux and web |

Alternatives considered:

- **Three.js / WebGPU:** a close second. It would be the better choice if
  shareable links mattered most. It's weaker for a packaged desktop game, and
  runtime Gemini calls would need a server-side proxy to keep API keys safe.
- **Unity:** capable, but gives no advantage here. It's hard to drive remotely
  and needs a license activated.
- **Bevy:** its UI support is too thin for a game this text-heavy.
- **Unreal:** far heavier than the game needs.

## Why AILANG as a sidecar (and not the other routes)

Evaluated against AILANG v0.45.0 on 2026-09-27:

| Route | Verdict | Reason |
|---|---|---|
| emit-go → `c-shared` → GDExtension | Not viable now | emit-go is frozen (`m-codegen-strategic-review.md`). Revisit if a native backend returns. |
| WASM inside Godot | Not viable | The build is `GOOS=js` and depends on the browser's `syscall/js`, so wasmtime/Godot can't host it. It is also about 33 MB. |
| Host Go functions via `[extensions]` | Not viable | `[extensions]` only bundles AILANG packages. Host code would mean forking AILANG. |
| **Sidecar over NDJSON stdio** | **Chosen** | Works today. Measured on this machine (below). |
| serve-api WebSocket route or `std/stream` client | Fallback | Works, but adds framing overhead and beats stdio at nothing. |

Measured in the spike (M4 Max):

- **Bytecode VM vs interpreter:** about 50× faster for the pure hot path.
  10k-record update: about 5 ms per tick with `--bytecode` (the pure map on the
  VM, I/O bridged), about 250 ms on the interpreter. The per-tick I/O costs
  about 30 µs whichever runtime handles it.
- **Round trip:** Godot → AILANG → Godot takes **about 50 µs per tick**.
- **Accuracy:** the ship kinematics match closed-form constant-acceleration
  results to 1e-12.
- **Parity:** VM and interpreter output is bit-identical over 600 ticks, and
  this is checked in CI with `make parity`.

This matches AILANG's own direction. `m-game-engine-effects.md` (approved
2026-07-14) makes a revived Stapledon's Voyage the v1.1 flagship, with the
logic in AILANG and rendering, input and clock as host effects. The NDJSON
protocol should be shaped so each message maps onto those planned effects,
which lets the game move to native effects later without a rewrite.

## Constraints and risks

- **VM correctness.** There are open bugs where the VM silently gives wrong
  results (`m-bytecode-vm-parity-bugs.md`). Mitigation: CI runs the simulation
  on both the VM and the interpreter and compares the results (`make parity`),
  and runs the pure core under `--strict-bytecode` (`make strict`). The game is
  meant to be a **stress test for the VM**: parity differences and bridged calls
  get reported upstream with minimal repros. When Phase 2E wires the I/O
  built-ins, the whole sidecar switches to `--strict-bytecode`.
- **Performance ceiling.** The interpreter and VM suit logic ticks, not inner
  loops per frame. Anything per-pixel or per-star-per-frame stays in Godot
  shaders. The 1M-year history is batched, not run inside the tick loop.
- **No host kernels.** Spatial queries over 100k stars and A* have to be pure
  AILANG, precomputed, or done in Godot and passed in as input. Lists are
  still O(n), so the simulation uses arrays and maps.
- **Blocking AI calls.** `std/ai` blocks, so Gemini dialogue runs in a second
  AILANG process or goes through Godot. It is never inside the simulation tick.
- **Random numbers.** `std/rand` is a single global seeded generator. Streams
  per civ or star that can be replayed need a pure hash-based generator
  (PCG/SplitMix) written in AILANG.
- **JSON.** There is no automatic encoding of records or ADTs, so codecs are
  written by hand. Keep the protocol small.
- **Toolchain churn.** Pin the AILANG version in CI. Report every bug or DX
  problem upstream with `ailang messages` (standing rule).

## Consequences

- The Go repo stays frozen as the original AILANG demo. Its design docs move
  here, and the Go-specific documents are kept under `legacy/`.
- New visuals must pass the physics spec and have tests before they merge.
- The simulation must be runnable and testable headless with no Godot at all:
  NDJSON in, NDJSON out.
