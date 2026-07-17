# MEMORY.md

## Session Log

### 2026-07-17 — bootstrap as an agentic host
- This repo is the host `agentic-calx-mill`; the software under development is
  `slartibardfast/calx-mill`. Adopted the methodology as case (a) (empty host).
  Template pinned at `host-template` submodule `8b578b2`; `.host` baseline `ff04a94`.
- `calx-mill` is embedded as a bare store with worktrees at `software/calx-mill/`,
  canonical worktree `software/calx-mill/main/` at pin `1f648db` (its `main` HEAD).
  Build `cargo build --release --locked`; 27 tests pass. Specs live in the worktree.
- `host-lint` is BOTH the `tools/host-lint` submodule (skills + reference) AND a
  `.host-software` component (so `software --install-hooks` builds the binary and
  installs `pre-commit`/`commit-msg`). Its `.allium` + Kani lanes self-discharge
  from host-lint's own repo files; this host does not re-wire them.
- The gate is green (`software --check` exit 0): both components at pin, artifacts
  verified, all eight phase receipts recorded (release for calx-mill is a skip).
- Commit identity is `David Connolly <david@connol.ly>`; pushes go through the
  `connollydavid` gh account (a collaborator on `slartibardfast` repos). Switch
  with `gh auth switch --user connollydavid`.

### Open constraints (worth knowing next session)
- No `rustup` on this machine; local Rust is `1.96.1`. Reproducible-build
  `toolchain` is recorded as `rust-1.96.1` (interim); the canonical
  `rust-1.95.0` container build is pending (see `call/0000`). Rust embeds the
  build path, so the host-lint hash differs between `tools/host-lint` and the
  `software/host-lint/main` worktree; record the worktree-relative hash.
- `calx-mill` is at Cargo version `0.1.0` with no `v0.1.0` git tag. The
  methodology treats an untagged version as a defect; the `release` phase for
  calx-mill is skipped pending an operator decision on back-filling `v0.1.0`.
- `cast/` (personas) is empty. The methodology asks for at least one persona
  built by discussion with the human before planning the work it serves.

### 2026-07-17 — v0.1.0 tagged (corrects the release skip above)
- Back-filled the `v0.1.0` annotated tag on `slartibardfast/calx-mill` at pin
  `1f648db` (its first release; the repo is fully tested and Kani-verified). The
  `release` phase for calx-mill is now recorded `done`; the earlier skip is
  superseded. calx-mill's worktree needed its own `user.name`/`user.email`
  (`David Connolly <david@connol.ly>`), set repo-local in
  `software/calx-mill/main`.

