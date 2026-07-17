# calx-mill persona modalities and their accommodations

- Status: accepted
- Scope: software
- Date: 2026-07-17

## Context and Problem Statement

calx-mill is consumed in two modalities. An embodied human operator runs it
interactively and verifies by intuition and against measurement; an agentic model
drives it programmatically and verifies by parsing deterministic output. An
interface designed for one cannot satisfy the other (Cooper 2004), so both
personas are primary. The cast lives in `cast/`; this decision sets the rule the
cast implies, so a later "why is there a structured surface alongside the
human-readable one?" has an answer.

## Decision

Treat the human operator (Soren) and the agentic model user (Vega) as the two
primary personas, split on modality. The substrate scenarios (a predicted-vs-
measured occupancy close on a GPU, extending the model to a new substrate, and
checking whether a workload fits a substrate's budget) are use-cases under each,
not separate personas. calx-mill owes each modality a distinct accommodation:

- **Human (Soren):** a README and CLI help that match the code; human-readable
  output that names the binding resource; runnable examples; a recorded,
  reproducible build.
- **Agentic (Vega):** a stable CLI surface; byte-deterministic structured output;
  explicit failure with no silent fallback to a looser model; machine-readable
  specs and recorded obligations it can discharge.

When the two pull in different directions, keep both surfaces. Do not collapse the
human-readable and the structured into one, and do not let one drift while serving
the other.

## Consequences

- Good: each modality gets a surface it can rely on, and a regression that hurts
  one is caught on its own terms.
- Neutral: two surfaces to keep in sync (prose and CLI help for Soren, structured
  output and specs for Vega), which is the cost of serving both.
- Risk: silent fallbacks and output drift are the shared failure mode, and both
  hurt Vega most, so they gate as explicit failures rather than warnings.
