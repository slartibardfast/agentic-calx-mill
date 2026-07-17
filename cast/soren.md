# Soren: the Human Operator

*The accountable developer-analyst who runs calx-mill by hand.* (Project persona, **primary**, embodied; see `call/0001`.)

**Modality: embodied and intermittent.** Soren perceives calx-mill through its README, its CLI help, and the numbers on a screen. He reads selectively, holds a durable but lossy mental model of the substrate that survives weeks away, and reasons slowly, cross-checking by intuition. He carries intent and accountability that live nowhere but in his head.

He runs the binary interactively (`project`, `census`, `check-sass`, `verify-projection`) against a kernel and a substrate, reads the projection, and judges whether it matches what he expects from the hardware. The substrate scenarios are his as much as anyone's:

- closing predicted occupancy to measured `ncu` and `cudaOccupancy` on a TU102 kernel;
- standing up a new substrate (an AVX-512 core, another GPU generation) by writing its resource axes;
- reasoning about whether a workload fits a substrate's register, local-store, and concurrency budget without a live measurement.

- **Goals:** trust a projection enough to act on it; understand why a number came out the way it did, not just the number; reproduce a run from the pinned source; catch a model that drifts from the silicon.
- **Frustrations:** a projection that silently disagrees with a measurement and gives no trace; output that shifts shape between releases; prose docs that lag the code; a model that works for one architecture and quietly lies for another.
- **Works by:** reading the README and the spec, running the CLI on a real kernel, and treating a mismatch between prediction and measurement as the thing to explain, never to tune away. The judgement that "this feels wrong" is his alone.

**Accommodations calx-mill owes Soren:** a README and CLI help that match the code, human-readable output that names the binding resource, runnable examples, and a recorded build he can reproduce.
