# Completion Report

## 2026-07-26 08:17:41 — Codex

Result: synchronized the current upstream mainline through the TeamADAPT fork
and into this checkout's `working` history without force-pushing, rebasing
published history, or dropping TeamADAPT commits.

## Git Receipts

- Remote `upstream/main`, remote `origin/main`, and local `main`:
  `8c6a945d9b5acbabd0b34f28809a066b179c0fad`.
- Integration merge:
  `598cb38191d24b68c8cbacae6396cb1503d45de3`.
- Integration parents:
  - TeamADAPT: `2e3d8a486690162d548c0f3d87174a5cbd73161e`
  - Upstream: `8c6a945d9b5acbabd0b34f28809a066b179c0fad`
- Local and remote `working` immediately before closeout:
  `7a91c3775e221d292c0b36410567210319d13e71`.
- The integration contains both parent histories.
- The 178-path integration inventory is in `merge_files.txt`.
- The tracked worktree was clean before closeout.

## Merge Resolution

Git reported four conflicts. The launcher template, UI Tweaks model-picker
implementation and test, and launcher smoke test were resolved byte-for-byte
to current upstream. Current upstream replaced the obsolete CLI-trust and old
DMG model-picker compatibility shapes while preserving their intended launch
and expanded-picker behavior.

No optional feature was enabled in committed configuration, and no legacy
compatibility branch was retained.

## Validation

Passing gates:

- 25 shell syntax checks.
- 54 focused UI Tweaks tests.
- 53 Python upstream-watchdog tests.
- Rust formatting, clippy, and check.
- 764 Rust tests.
- Live GitHub ref and ancestry checks.

Reported current-upstream failures:

- Full Node lane: 1,369 passed; three directory-watch timer/backoff assertions
  failed; one test was cancelled at the 300-second watchdog; two skipped.
- Script smoke reached the host-specific Linuxbrew fixture assertion, where
  this host's real `/usr/bin/codex` invalidates the test's hard-coded
  `/usr/bin:/bin` clean-PATH assumption.

The failing implementation and tests are byte-identical to
`upstream/main` and were not part of TeamADAPT conflict resolution. Full
commands, assertions, unavailable toolchains, and scope caveats are recorded
in `validation_report.md`.

## Installed Application Scope

This task synchronized Git; it did not rebuild or install a new desktop
package. At the user's request, the old `/opt/codex-desktop` process tree was
terminated. GNOME Shell subsequently launched the installed app again from the
user session. That old installed package is not represented as a runtime
receipt for the merged source.

The local-only `dev/mobile-gpu-route` branch was left unchanged because the
approved current plan scoped local integration to `main` and the checked-out
`working` branch.

**— Codex**
