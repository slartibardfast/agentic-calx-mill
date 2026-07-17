# 0001 lacunae-remediation

Promote the lacunae-remediation line (branch `lacunae-remediation` @ f11bb05,
fast-forwardable from `main` @ 1f648db) to calx-mill `main`, and re-pin this host
to it. The line delivers the gate exit-code contract v2 (0 CERTIFIED, 3
PROVISIONAL, 2 REFUSED, 4 usage; PROVISIONAL no longer exits 0), instrument and
tensor-pipe hardening, strict-mode parsers, and the lacunae review. It is the
subject of calx-mill issue #1.

- **Who**: both primaries. Soren reads the new gate output and the instrument
  columns; Vega is the reason the contract is stable and explicit (see
  `call/0001`), since a non-zero PROVISIONAL is the agent-facing accommodation.
- **What**: the gate contract is exercised in the calx-mill worktree
  (`tests/gate_cli.rs`), not under a host `spec/`; the host `plan/` only
  references it.
- **Why**: `call/0001` (Vega needs explicit, stable failure). The consumer sweep
  in issue #1 is a verified no-op (no consumer keys on the old exit codes), so
  the contract change breaks nothing here.

The canonical container artifact hash (`rust@sha256:44637ff2…`, rootless podman)
is deferred to `call/0000` (no podman on this host); the hash recorded against the
new pin is the local rust-1.96.1 build, with the container hash pending.

## Build sequence

### Verify the branch {#verify-lacunae}

- verify: cargo test --release --locked --manifest-path software/calx-mill/main/Cargo.toml && cargo kani --manifest-path software/calx-mill/main/Cargo.toml
- inputs: software/calx-mill/main/Cargo.toml software/calx-mill/main/src software/calx-mill/main/tests

### Promote main {#promote-main}

- depends: #verify-lacunae
- verify: test "$(git -C software/calx-mill/main rev-parse main)" = "f11bb05602a8e6ba7e892cc90db82cc417468d03"

### Re-pin the host {#repin}

- depends: #promote-main
- verify: host-lifecycle software --check .
