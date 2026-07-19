# host-lint canonical verify lane

- Status: accepted
- Scope: software
- Date: 2026-07-19

## Context and Problem Statement

`call/0002` wired calx-mill's canonical container toolchain but left host-lint
ambient, its `toolchain` recorded as `rust-1.96.1`. That value is not an OCI
image, so `software --verify-build` tried to run an image named `rust-1.96.1`
and ERRORed. host-lint is a `.host-software` component here because its commit
hooks install from it, so its verify lane is in scope and should not error.

## Decision

Record host-lint's `toolchain` as the digest-pinned OCI image
`docker.io/library/rust@sha256:443dd9a3260cf23c22fc05051dd5661dd7b4028d3d25dbaffab6563b63c3539c`
(rustc 1.95.0, matching host-lint's own `rust-toolchain.toml`), with
`build = cargo build --release --locked`. The build fetches `host-grammar` from
GitHub, so the verify container runs with network (no `deps-bundle` yet).

The canonical artifact hash for pin `241a870` (upstream `v0.14.2`) is
`447e19792667e933007bf0c3bcf734f7a86aaa3ad0a7afc7056f4a4ea0392e5b`.
`software --verify-build` reproduces it in the image. Both components now carry a
canonical anchor, so `call/0000`'s interim stance is fully retired.

## Consequences

- Good: `software --verify-build` across both components is green (2 reproduced,
  0 deferred, 0 exempt); host-lint's hook binary is the canonical build, verified
  against the recorded hash.
- Neutral: host-lint's verify build needs network for `host-grammar`; a future
  hermetic form would vendor it as a `deps-bundle`.
- Risk: the canonical hash tracks the pinned image digest, as with calx-mill.
