# Validation Report

## 2026-07-15 02:38:34 — Codex

Passing validation:

- Shell syntax: `launcher/start.sh.template` and every tracked `*.sh` file.
- Node syntax, label policy, patch registry, and Linux feature suite:
  1,110 passed, 0 failed.
- Rust workspace coverage: 721 tests passed when the updater suite was run
  serially after the broad workspace run exposed the concurrency issue below.
- Standalone Rust crates: Global Dictation 8 passed; MCP Helper Reaper 25
  passed.
- Rust formatting: workspace, Global Dictation, and MCP Helper Reaper passed
  `cargo fmt --check`.
- Python upstream DMG watchdog: 53 passed, 0 failed.
- `tests/scripts_smoke.sh`: passed in isolation, covering Debian, RPM, pacman
  hooks, AppImage, launcher, updater, feature staging, and candidate promotion.
- Nix pins: current upstream DMG version `26.707.72221`, Electron `42.1.0`,
  native module pins, and Browser Use runtime pins all match.

Reported validation defect:

- `cargo test --workspace` failed one updater test after 260 updater tests
  passed: `codex_cli::tests::preflight_uses_cached_latest_for_fresh_explicit_cli_path`.
- Repeating `cargo test -p codex-update-manager` with normal concurrency
  reproduced the same single failure.
- The failing test passed alone and the complete 261-test updater suite passed
  with `--test-threads=1`. This is an upstream test environment isolation race,
  not a merge conflict or source resolution introduced by this synchronization.
- The first parallel smoke attempt collided with a simultaneous MCP Helper
  Reaper Cargo build changing its ignored `target/` tree. The isolated smoke
  rerun passed; its result is the authoritative receipt.

Live synchronization receipts after validation:

- Remote `upstream/main`: `02c16466691c6082113065bc621ce32d4c105c9d`.
- Remote `origin/main`: `02c16466691c6082113065bc621ce32d4c105c9d`.
- Local `main`: `02c16466691c6082113065bc621ce32d4c105c9d`.
- Remote `origin/working`: `746a4f28bc561ab6c1e24d28152176fa6e4dbc42`.
- Local `working`: `746a4f28bc561ab6c1e24d28152176fa6e4dbc42`.
- Local `dev/mobile-gpu-route`: `746a4f28bc561ab6c1e24d28152176fa6e4dbc42`.
- Worktree: clean before this report was written.

Generated test outputs remain ignored under `target/`, standalone crate
`target/` directories, and Python `__pycache__/` directories.

**— Codex**
