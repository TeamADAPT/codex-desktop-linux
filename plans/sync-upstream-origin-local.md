# Upstream To Origin To Local Sync Plan

## 2026-07-15 02:22:59 — Codex

Objective: synchronize the newest upstream `main` through the TeamADAPT
`origin` fork and into the local development line without rewriting published
history or discarding local work.

Verified starting state:

- Worktree is clean.
- `upstream/main` is `52e9701e3f1be291821cff904b6cd4bdce30998d`.
- `origin/main` and local `main` are
  `fa5baa3896d38c7bfa98eee4875bd973c62bca05`.
- `origin/working`, local `working`, and local `dev/mobile-gpu-route` are
  `6bcbc6334823ec974d951992cd29ad62a9312faa`.

Execution:

1. Obtain user approval for this plan before changing source refs or files.
2. Fetch and prune `upstream` and `origin`.
3. Require `origin/main` to be an ancestor of `upstream/main`. Stop rather than
   force-push if the histories diverge.
4. Push `upstream/main` to `origin/main` as a normal fast-forward, then update
   local `main` from the synchronized `origin/main`.
5. Merge synchronized `origin/main` into `working` without rewriting the
   published `origin/working` history. Resolve any conflicts in favor of the
   repository's current architecture and retained TeamADAPT work.
6. Fast-forward local `dev/mobile-gpu-route` to the updated `working` commit
   only if the branch has not moved independently.
7. Validate the resulting commit graph and run tests selected from the actual
   upstream diff. Prefer live integration receipts where applicable.
8. Record receipts, write the completion report, move the task to
   `ops/completed/`, commit each meaningful step with an agent signature, and
   push `working`.

Success receipts:

- Remote `upstream/main`, remote `origin/main`, and local `main` resolve to the
  same upstream commit.
- `working` contains both the prior TeamADAPT tip and the synchronized upstream
  tip.
- The worktree is clean and `origin/working` matches local `working`.
- Validation selected from the merged diff passes, or any failure is reported
  explicitly with its command and output summary.

Execution progress:

- 2026-07-15 02:38:34 MST: completed broad native validation and reconfirmed
  the live remote refs. Shell, Node (1,110 tests), isolated package smoke,
  Rust formatting, Python watchdog (53 tests), Nix pins, and 754 Rust tests
  passed. Normal-concurrency updater testing reproducibly exposes one upstream
  test-isolation failure that passes alone and in the complete serial suite;
  details are recorded in the task validation report.
- 2026-07-15 02:33:54 MST: verified local `dev/mobile-gpu-route` had not
  moved from its recorded `6bcbc63` starting point and fast-forwarded it to
  the integrated `working` history.
- 2026-07-15 02:33:27 MST: merged synchronized `origin/main` into `working`
  as `ff98fa4` with parents `12d15c5` and `02c1646`. Git's `ort` strategy
  completed without conflicts; the upstream side changed 400 paths.
- 2026-07-15 02:32:44 MST: fast-forwarded local `main` to tracked
  `origin/main`; local `main`, remote `origin/main`, and remote
  `upstream/main` now resolve to `02c1646`.
- 2026-07-15 02:32:20 MST: fast-forwarded remote `origin/main` from `fa5baa3`
  to `02c1646`; direct remote reads confirm `origin/main` and `upstream/main`
  match exactly.
- 2026-07-15 02:31:53 MST: fetched and pruned both remotes.
- The upstream tip advanced after initial planning from `52e9701` to
  `02c16466691c6082113065bc621ce32d4c105c9d`; a direct `ls-remote` confirmed
  `02c1646` is the current upstream `main`.
- Verified `origin/main` at `fa5baa3` is an ancestor of `upstream/main`; the
  fast-forward gate passed with 448 upstream commits pending.

Status: completed on 2026-07-15 at 02:39:27 MST.

**— Codex**
