# Upstream To Origin To Local Sync Plan

## 2026-07-26 07:45:54 — Codex

Objective: synchronize the current upstream `main` through the TeamADAPT
`origin` fork and into this checkout's `working` branch without rewriting
published history or discarding TeamADAPT work.

Approval status: approved explicitly by the user on 2026-07-26 at 07:54:30
MST. Execution is authorized under this dated plan.

Verified read-only starting state:

- The worktree is clean on local `working`.
- Direct server read: `upstream/main` is
  `8c6a945d9b5acbabd0b34f28809a066b179c0fad`.
- Direct server read: `origin/main` is
  `02c16466691c6082113065bc621ce32d4c105c9d`.
- Direct server read: `origin/working`, local `working`, and `HEAD` are
  `1c2d96ce2be0e1ab482daeedde27a6c217d7bf88`.
- Local `main` is `02c16466691c6082113065bc621ce32d4c105c9d`.
- The cached `upstream/main` ref is stale, so ancestry and the exact incoming
  diff must be established from a fresh fetch after approval.

Execution:

1. Commit and push this planning record, then obtain explicit user approval
   for this dated plan before changing synchronization refs or source files.
2. Move `ops/to_do/sync_upstream_origin_local_20260726/` to
   `ops/in_progress/`, record the approval, commit, and push the task start.
3. Fetch and prune `upstream` and `origin`.
4. Reconfirm a clean worktree, unchanged starting refs, and require
   `origin/main` to be an ancestor of `upstream/main`. Stop rather than
   force-push if the histories diverge or either server ref moves during the
   operation.
5. Push the freshly fetched `upstream/main` commit to `origin/main` as a
   normal fast-forward and verify both server-side refs directly.
6. Fast-forward local `main` to the synchronized `origin/main`.
7. Merge synchronized `origin/main` into `working` without rewriting the
   published `origin/working` history. Resolve any conflicts in favor of the
   current repository architecture while retaining TeamADAPT changes.
8. Validate the resulting graph and run the relevant repository checks chosen
   from the actual incoming diff. Use live GitHub remote reads as the
   synchronization receipts and prioritize live system integration checks if
   the merged paths affect runnable behavior.
9. Push `working`, verify local and remote commit IDs, write the completion
   report, move the task to `ops/completed/`, commit, and push the closeout.

Stop conditions:

- The worktree or tracked branches move unexpectedly.
- `origin/main` is not an ancestor of freshly fetched `upstream/main`.
- A normal non-force push cannot update `origin/main`.
- A conflict cannot be resolved directly from repository sources, plans,
  knowledge, and history without changing intended TeamADAPT behavior.
- Required validation exposes a failure caused by the integration.

Success receipts:

- Remote `upstream/main`, remote `origin/main`, and local `main` resolve to the
  same commit.
- `working` contains both its recorded TeamADAPT starting tip and the
  synchronized upstream tip.
- Local `working` and remote `origin/working` resolve to the same commit.
- The worktree is clean.
- Relevant validation passes, or any failure is reported immediately with its
  command and evidence.

Execution progress:

- 2026-07-26 07:56:19 MST: fast-forwarded local `main` from `02c1646` to
  tracked `origin/main` at `8c6a945`, then returned to the unchanged `working`
  branch. Local `main`, `origin/main`, and `upstream/main` now resolve to the
  same commit.
- 2026-07-26 07:55:51 MST: re-read both server refs and revalidated ancestry,
  then fast-forwarded `origin/main` from `02c1646` to `8c6a945` with a normal
  push. Refetched `origin/main`; fetched refs and direct server reads now
  confirm `upstream/main` and `origin/main` match exactly at `8c6a945`.
- 2026-07-26 07:55:15 MST: fetched and pruned both remotes. Fresh refs show
  `origin/main` is an ancestor of `upstream/main` with divergence `0 180`;
  the required no-force gate passed. The incoming mainline delta changes 178
  paths with 28,160 insertions and 5,613 deletions. Direct server reads still
  match the fetched `upstream/main` and `origin/main`, while
  `origin/working` matches the approved task-start commit `78211b3`.
- 2026-07-26 07:54:30 MST: received explicit user approval, moved the queued
  task into `ops/in_progress/`, and started execution. Direct server reads
  immediately before task start confirmed `upstream/main` remained `8c6a945`,
  `origin/main` remained `02c1646`, and `origin/working` matched the planning
  commit `01a6faf`.

Status: approved and in progress.

**— Codex**

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
