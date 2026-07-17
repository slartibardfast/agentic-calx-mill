# Cast: the project's Who

Personas are hypothetical archetypal actors that keep the work grounded in the
people it serves. The workshop process is in [applying-personas.md](applying-personas.md);
its story steps become `plan/` milestones, and a scenario a persona hits becomes
acceptance criteria under a milestone's spec. (Powell, Keenan & McDaid 2007,
after Cooper & Reimann, Interaction Design.)

calx-mill is consumed in two modalities that need different accommodations, so
the cast splits on modality rather than on substrate. The substrate scenarios
(a predicted-vs-measured occupancy close on a GPU, extending the model to a new
substrate, and checking whether a workload fits a substrate's budget) are
use-cases both personas engage. What differs is how each can engage them, and
therefore what calx-mill owes each.

## This project's cast

- [Soren](soren.md), the human operator. **Primary** (embodied, intermittent):
  runs calx-mill interactively, reads the docs, verifies by intuition and against
  measurement.
- [Vega](vega.md), the agentic model user. **Primary** (textual, ephemeral):
  drives the CLI programmatically, needs stable deterministic structured output
  and explicit failure.

Both are primary because an interface designed for one cannot satisfy the other
(Cooper 2004): a human-facing surface (prose, examples, readable output) does not
serve an agent, and a stable structured surface does not serve a human reading by
eye. So calx-mill owes each its own accommodation, set as a rule in `call/0001`.

## The shared substrate

Both operate the same model. The substrate facts every persona and spec must
respect:

- calx-mill generalizes the Roofline model (Williams, Waterman, Patterson 2009)
  into a multi-pipe, multi-resource model with an explicit concurrency dimension;
  "occupancy" is concurrency saturation, generic across substrates.
- The core is Kani-verified universal over substrate specs: concurrency is bounded
  by the ceiling, allocation rounding is the tightest valid multiple, occupancy
  stays in range, and fewer per-unit resources never lowers concurrency.
- The empirical anchor (the runtime `cudaOccupancy*` API, `ncu` achieved occupancy
  on a compiled kernel) stays outside Kani: measured truth, not proven. The NVIDIA
  adapter under `src/nvidia/` ports the TU102 / `sm_75` measurement pipeline and
  is held to byte-for-byte parity against its Python goldens.

A gap a persona hits becomes acceptance criteria under a milestone's spec; the
modality rule (`call/0001`) decides which accommodation applies.
