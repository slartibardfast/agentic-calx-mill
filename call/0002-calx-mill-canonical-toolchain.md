# calx-mill canonical reproducible-build toolchain

- Status: accepted
- Scope: software
- Date: 2026-07-19

## Context and Problem Statement

`call/0000` recorded calx-mill's build toolchain as interim local rust-1.96.1,
with the canonical container hash pending a podman-equipped environment. That
environment now exists (rootless podman 6.0.1 on this host), so the canonical
anchor can be established. The methodology's canonical hash is the digest-pinned
container's output, not a host-ambient build: host glibc, the linker, and the
source paths the compiler embeds all differ.

## Decision

Record calx-mill's `toolchain` as the digest-pinned OCI image
`docker.io/library/rust@sha256:44637ff22d0a6571a221bfaf137849711ad02ff4723dbb4736e297538f6a3e60`
(rustc 1.97.0), with `build = cargo build --release --locked`. The host-lifecycle
verify lane mounts the worktree at a fixed `/src` inside the container, so the
source path the compiler embeds is normalised across rebuilds and the artifact is
byte-reproducible without `--remap-path-prefix`.

The canonical artifact hash for pin `f11bb05` is
`17a3799e21b7a7a12168a65d1d3fb19935ad1308920b5021d1b1b1c86cf2045f`.
`host-lifecycle software --verify-build --item calx-mill` rebuilds in the image
and reproduces it.

This retires the interim stance in `call/0000` for calx-mill. The gating tool
`host-lint` stays ambient, a consumed tool whose own repo verifies it (this host
only installs its hooks), so `call/0000` remains in force for host-lint.

## Consequences

- Good: calx-mill's pin is now a true production anchor; a clean rebuild from the
  pin in the pinned container reproduces the deployed bytes, proven by the verify
  lane rather than asserted.
- Neutral: a host-ambient `cargo build` in the dev worktree still differs from the
  canonical hash; `software --check` reports that as a noted difference, not a
  failure. The canonical proof is `--verify-build`.
- Risk: the canonical hash is only as stable as the pinned image digest; bumping
  the image re-derives the hash through `--verify-build` and re-records it.
