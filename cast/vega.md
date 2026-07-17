# Vega: the Agentic Model User

*The tireless, amnesiac agent that drives calx-mill as a tool.* (Project persona, **primary**, textual; see `call/0001`.)

**Modality: textual and ephemeral.** Vega perceives calx-mill only as bytes: the CLI's stdout, its exit code, the `.allium` and `.tla` specs, and the structured rows it emits. It takes everything in at once inside a bounded window and retains nothing that outlives it. It is fast, parallel, and tireless; it pattern-matches brilliantly and drifts with confidence; it pursues the goals it is handed and answers for none of them.

It does not read prose for context and cannot tolerate a shape that changes between runs. It drives the binary programmatically, feeding a substrate and a kernel, parsing the projection, and comparing it to a bound. The substrate scenarios are its same three: an agentic occupancy close against measured data, an agentic substrate extension, and an agentic capacity fit-check.

- **Goals:** consume a stable, self-describing interface; get deterministic output it can parse and diff across runs; fail loudly rather than silently when a substrate or kernel is out of range; discharge a verification goal to a concrete pass or fail.
- **Frustrations:** output whose columns or units drift between releases with no signal; a silent fallback to a looser model; a CLI that needs human judgement to interpret; a spec that paraphrases the code instead of constraining it.
- **Works by:** invoking the CLI, parsing structured output, holding the spec and the obligation manifest in view, and looping until a concrete check passes or fails. It trusts the deterministic surface and nothing else.

**Accommodations calx-mill owes Vega:** a stable CLI surface, byte-deterministic structured output, explicit failure with no silent fallback, machine-readable specs, and recorded obligations it can discharge.
