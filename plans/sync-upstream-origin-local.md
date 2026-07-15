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

Status: approved by the user on 2026-07-15; execution started at 02:31:25 MST.

**— Codex**
