# Validation Report

## 2026-07-26 08:16:22 — Codex

Result: the live Git synchronization graph is correct and the native Rust,
focused conflict-resolution, shell-syntax, and watchdog gates pass. The full
Node and script-smoke lanes expose current-upstream failures documented below;
none is introduced by the TeamADAPT merge resolution.

## Live Git Receipts

- Direct `upstream/main`:
  `8c6a945d9b5acbabd0b34f28809a066b179c0fad`.
- Direct `origin/main`:
  `8c6a945d9b5acbabd0b34f28809a066b179c0fad`.
- Local `main`:
  `8c6a945d9b5acbabd0b34f28809a066b179c0fad`.
- Direct `origin/working` and local `working` before this report commit:
  `37dd1802339ad2cd7193cb870d44cdd2f900dd3e`.
- Integration merge:
  `598cb38191d24b68c8cbacae6396cb1503d45de3`.
- Merge parents: TeamADAPT `2e3d8a4` and upstream `8c6a945`.
- Both the pre-merge TeamADAPT tip and synchronized upstream tip are ancestors
  of `working`.
- Tracked worktree was clean before this report was written.

These values were read from GitHub with `git ls-remote`, not inferred only
from cached local refs.

## Passing Validation

- Merge integrity:
  - Four conflict resolutions are byte-identical to the corresponding
    `main` blobs.
  - No conflict markers or removed compatibility identifiers remain.
  - `git diff --check` passed.
- Shell syntax: 25 of 25 launcher, install, package-builder, library, and CI
  shell checks passed.
- Focused UI Tweaks: 54 of 54 Node tests passed.
- Upstream DMG watchdog: 53 of 53 Python tests passed.
- Rust:
  - `cargo fmt --check` passed.
  - `cargo clippy --workspace --all-targets -- -D warnings` passed.
  - `cargo check --workspace --all-targets` passed.
  - `cargo test --workspace --all-targets` passed: 764 tests, zero failures.
- Script smoke completed all earlier Debian, RPM, pacman, AppImage, candidate
  promotion, native setup, Nix refresh, native-module, bundled-plugin,
  Browser Use, Chrome, warm-start recovery, and resident-reopen stages before
  the host-specific CLI lookup assertion described below.

## Reported Upstream Test Failures

### Full Node Lane

Command:

`bash scripts/ci/run-node-checks.sh`

Result:

- Exit `124` at the configured 300-second watchdog.
- 1,375 tests observed: 1,369 passed, 3 failed, 1 cancelled, 2 skipped.
- Three failures came from the newly integrated, optional
  `directory-only-working-tree-watch` feature:
  - `transient Git spawn resource failures retain metadata targets and retry`
  - `Git refresh watch resource failures retain their recovery timer`
  - `capacity released during recovery is replayed after the in-flight scan`
- The timeout cancelled `scripts/dev/upstream-dmg-intel.test.js` while its
  promise remained pending.
- An isolated rerun of
  `linux-features/directory-only-working-tree-watch/test.js` completed 112
  tests with 110 passing and the first two timer/backoff assertions failing;
  the capacity-replay assertion passed in isolation.

The feature implementation and tests are byte-identical to current
`upstream/main`, were not involved in a TeamADAPT conflict, and the feature
remains disabled by default. These are reported current-upstream test defects,
not rewritten during a synchronization-only task.

### Script Smoke Lane

Command:

`bash tests/scripts_smoke.sh`

Result:

- Exit `1` after approximately 310 seconds.
- Failure:
  `CLI lookup must find Linuxbrew installs with a GUI PATH, got /usr/bin/codex`.

The current upstream test hard-codes `/usr/bin:/bin` as a supposedly clean GUI
path. This host has a real `/usr/bin/codex`, so the launcher's documented
first-PATH-hit behavior selects it before the synthetic Linuxbrew fixture.
Both `tests/scripts_smoke.sh` and `launcher/start.sh.template` are
byte-identical to `upstream/main`; the failure is a host-fixture assumption,
not an integration conflict.

## Unavailable Or Out-Of-Scope Lanes

- Real Nix, RPM, pacman, and AppImage builds were not run because this Ubuntu
  host lacks `nix`, `rpmbuild`, `makepkg`, and `appimagetool`.
- Container substitution was not used because repository policy forbids
  Docker.
- No fresh-DMG candidate was installed or launched. The requested operation
  was Git synchronization, and the user separately requested termination of
  the old installed app. Therefore the existing installed package is not
  represented as a receipt for the merged source.

## Process-Termination Receipt

- At the user's request, sent `SIGTERM` to the exact
  `/opt/codex-desktop` process set, including launcher, webview, Electron,
  crash handler, and bundled `node_repl` processes.
- The initial 19 matching processes exited and loopback port 5175 stopped
  accepting connections.
- A later launch originated directly from the GNOME Shell session rather than
  an enabled Codex systemd user service. The replacement transient scope and
  process tree were stopped again.
- The user then indicated the shutdown issue was handled. No service was
  disabled and no package-managed file was renamed or deleted.
- The user-session app was subsequently launched again; that running old
  package is intentionally not treated as merged-source validation.

**— Codex**
