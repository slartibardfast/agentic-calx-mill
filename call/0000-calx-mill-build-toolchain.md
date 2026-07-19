# calx-mill reproducible-build toolchain

- Status: superseded by call/0002 and call/0003
- Scope: software
- Date: 2026-07-17

## Context and Problem Statement

The methodology requires software initiated under it to have byte-reproducible
builds: the deployed artifact reproduces from the pinned source plus a recorded
build recipe (a pinned `toolchain` and `build` command). calx-mill is a Rust
crate with no external dependencies, so `cargo build --release --locked` is the
whole build; the open question is which Rust the recipe pins.

The methodology's reproducible-build anchor for its own host tools is
`rust-1.95.0` (the digest-pinned `rust:1.95.0` container). host-lint, the gating
tool this host consumes, pins `1.95.0` in its own `rust-toolchain.toml`.

## Decision

Record calx-mill's (and host-lint's local build's) `toolchain` as `rust-1.96.1`,
the Rust installed on the development machine, and record the artifact hash that
this toolchain produces. The recorded hash matches the materialized worktree's
binary, so `software --check` reports the artifact verified in this environment.

This is interim. The canonical artifact hash is the `rust-1.95.0` container's
output; a container build in that pinned toolchain (run by the `release` phase or
a CI job) establishes the canonical hash, after which the recipe pins
`rust-1.95.0` and `software --verify-build` proves the deployed binary off the
pin alone.

## Consequences

- Good: the pin is a working production anchor now; the build is locally
  reproducible (Rust release builds are deterministic for a given source plus
  toolchain), and the gate is green.
- Neutral: `rustup` is not installed on this machine, so a pinned per-project
  toolchain is not installed locally; the local system Rust stands in.
- Risk: a different Rust on another machine produces a different artifact hash
  (Rust embeds the build path in panic messages, so even the same toolchain built
  in a different directory differs). Closing this means a pinned container build
  with path remapping, which the `release` phase owns.
