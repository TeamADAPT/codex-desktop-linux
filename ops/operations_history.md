# Operations History

## 2026-07-15 03:15:25 — Codex
Started the ChatGPT Work mode review.

Actions performed:

- Moved the task from `ops/to_do/` to `ops/in_progress/`.
- Retained the read-only scope and began evidence collection against upstream and the live installation.

Files touched:
- `ops/to_do/review_chatgpt_work_mode/`
- `ops/in_progress/review_chatgpt_work_mode/`
- `ops/operations_history.md`
- `ops/decisions.log`

**— Codex**

## 2026-07-15 03:15:08 — Codex
Selected a focused read-only review task for the newly released ChatGPT Work mode.

Actions performed:

- Created the task record under `ops/to_do/`.
- Scoped the work to upstream source, official release notes, the official DMG payload, and live Linux state.
- Prohibited application source and runtime configuration changes for this review.

Files touched:
- `ops/to_do/review_chatgpt_work_mode/task.md`
- `ops/operations_history.md`
- `ops/decisions.log`

**— Codex**

## 2026-07-15 02:39:27 — Codex
Completed the upstream-to-origin-to-local synchronization task with final live Git receipts.

Actions performed:

- Re-read remote `upstream/main`, remote `origin/main`, and remote `origin/working` directly from GitHub.
- Verified upstream, fork, and local mainlines match at `02c16466691c6082113065bc621ce32d4c105c9d`.
- Verified remote `origin/working`, local `working`, and local `dev/mobile-gpu-route` matched at `13625b9118dbede5916d6547a03deb635f1c63ab` before the closeout documentation commit.
- Verified `working` contains both local `main` and the original TeamADAPT tip `6bcbc63`.
- Verified the worktree was clean.
- Wrote the completion report, marked the approved plan complete, and moved the task from `ops/in_progress/` to `ops/completed/`.

Files touched:
- `plans/sync-upstream-origin-local.md`
- `ops/in_progress/sync_upstream_origin_local/`
- `ops/completed/sync_upstream_origin_local/`
- `ops/operations_history.md`
- `ops/decisions.log`

**— Codex**

## 2026-07-15 02:38:34 — Codex
Completed broad native validation of the synchronized history and reconfirmed the live remote and local refs.

Actions performed:

- Passed shell syntax for the launcher template and every tracked shell script.
- Passed the complete Node syntax/test matrix: 1,110 tests, 0 failures.
- Passed 721 Rust workspace tests when the updater suite was serialized, plus 8 Global Dictation and 25 MCP Helper Reaper tests.
- Passed Rust formatting checks for the workspace and standalone Rust crates.
- Passed 53 Python upstream DMG watchdog tests.
- Passed the isolated full scripts smoke suite across Debian, RPM, pacman hooks, AppImage, launcher, updater, feature staging, and candidate promotion.
- Passed Nix pin validation against upstream DMG `26.707.72221` and Electron `42.1.0`.
- Reconfirmed direct live refs: `upstream/main`, `origin/main`, and local `main` match at `02c1646`; `origin/working`, local `working`, and local `dev/mobile-gpu-route` matched at `746a4f2` before this report commit.
- Recorded the reproducible normal-concurrency updater test isolation failure and the invalidated concurrent smoke attempt in `ops/in_progress/sync_upstream_origin_local/validation_report.md`.

Files touched:
- `target/` (ignored Rust build output)
- `global-dictation-linux/target/` (ignored Rust build output)
- `linux-features/mcp-helper-reaper/reaper/target/` (ignored Rust build output)
- `launcher/__pycache__/` (ignored Python bytecode)
- `scripts/automation/upstream-dmg-watchdog/__pycache__/` (ignored Python bytecode)
- Temporary test directories under `/tmp/` (removed by test cleanup)
- `plans/sync-upstream-origin-local.md`
- `ops/in_progress/sync_upstream_origin_local/validation_report.md`
- `ops/operations_history.md`
- `ops/decisions.log`

**— Codex**

## 2026-07-15 02:33:54 — Codex
Fast-forwarded the original local development branch to the integrated `working` history after verifying it had not moved independently.

Actions performed:

- Verified `dev/mobile-gpu-route` still resolved to its recorded starting commit `6bcbc63`.
- Verified `dev/mobile-gpu-route` was an ancestor of `working`.
- Fast-forwarded local `dev/mobile-gpu-route` to the integrated `working` history without force-pushing or creating a remote branch.

Files touched:
- `.git/refs/heads/dev/mobile-gpu-route`
- `plans/sync-upstream-origin-local.md`
- `ops/operations_history.md`
- `ops/decisions.log`

**— Codex**

## 2026-07-15 02:33:27 — Codex
Merged the synchronized upstream mainline into the TeamADAPT `working` branch without rewriting either history.

Actions performed:

- Created merge commit `ff98fa450c52e8fa27ba25996b69600fbe5923b3` with TeamADAPT parent `12d15c5` and upstream parent `02c1646`.
- Git's `ort` merge strategy completed without conflicts or manual source edits.
- Integrated the upstream delta across 400 paths: 85,028 insertions and 10,390 deletions relative to the prior `working` tip.
- Captured the complete merged-path inventory in `ops/in_progress/sync_upstream_origin_local/merge_files.txt`.
- Read the newly integrated `AGENTS.md` and retained the user-provided ops and approval requirements governing this active task.

Files touched:
- `.git/index`
- `.git/ORIG_HEAD`
- `.git/MERGE_MSG`
- `.git/refs/heads/working`
- All 400 repository paths enumerated in `ops/in_progress/sync_upstream_origin_local/merge_files.txt`
- `ops/in_progress/sync_upstream_origin_local/merge_files.txt`
- `plans/sync-upstream-origin-local.md`
- `ops/operations_history.md`
- `ops/decisions.log`

**— Codex**

## 2026-07-15 02:32:44 — Codex
Fast-forwarded local `main` from the synchronized TeamADAPT fork mainline.

Actions performed:

- Revalidated that local `main` was an ancestor of `origin/main`.
- Fast-forwarded local `main` from `fa5baa3` to `02c1646` and retained tracking of `origin/main`.
- Verified local `main`, `origin/main`, and `upstream/main` all resolve to `02c16466691c6082113065bc621ce32d4c105c9d`.
- Left the `working` worktree content unchanged pending the planned integration merge.

Files touched:
- `.git/refs/heads/main`
- `.git/config`
- `plans/sync-upstream-origin-local.md`
- `ops/operations_history.md`
- `ops/decisions.log`

**— Codex**

## 2026-07-15 02:32:20 — Codex
Fast-forwarded the TeamADAPT fork's remote mainline to the current upstream mainline.

Actions performed:

- Refetched `upstream/main` and `origin/main` immediately before the update.
- Revalidated that `origin/main` was an ancestor of `upstream/main`.
- Pushed `upstream/main` to `origin/main` as a normal fast-forward from `fa5baa3` to `02c1646`; no force option was used.
- Refetched `origin/main` after the push.
- Verified direct remote reads return `02c16466691c6082113065bc621ce32d4c105c9d` for both `upstream/main` and `origin/main`.

Files touched:
- `.git/FETCH_HEAD`
- `.git/refs/remotes/origin/main`
- `plans/sync-upstream-origin-local.md`
- `ops/operations_history.md`
- `ops/decisions.log`

**— Codex**

## 2026-07-15 02:31:53 — Codex
Fetched both remotes and passed the required non-force ancestry gate for the mainline synchronization.

Actions performed:

- Fetched and pruned `upstream` and `origin`.
- Observed that upstream advanced after initial planning from `52e9701` to `02c1646`.
- Confirmed the current remote `upstream/main` directly with `git ls-remote`.
- Verified `origin/main` at `fa5baa3` is an ancestor of `upstream/main` at `02c1646`.
- Counted 448 upstream commits pending on `origin/main`; no divergence or force update is required.
- Left `origin/main`, local `main`, and source files unchanged during this verification step.

Files touched:
- `.git/FETCH_HEAD`
- `.git/refs/remotes/upstream/`
- `plans/sync-upstream-origin-local.md`
- `ops/operations_history.md`
- `ops/decisions.log`

**— Codex**

## 2026-07-15 02:31:25 — Codex
Started the approved upstream-to-origin-to-local synchronization task.

Actions performed:

- Received explicit user approval for `plans/sync-upstream-origin-local.md`.
- Updated the plan status from awaiting approval to approved and in execution.
- Moved the task directory from `ops/to_do/` to `ops/in_progress/`.

Files touched:
- `plans/sync-upstream-origin-local.md`
- `ops/to_do/sync_upstream_origin_local/task.md`
- `ops/in_progress/sync_upstream_origin_local/task.md`
- `ops/operations_history.md`
- `ops/decisions.log`

**— Codex**

## 2026-07-15 02:29:49 — Codex
Repaired GitHub CLI and Git HTTPS authentication for the `ADAPT-Chase` account using the repository owner's existing secret source.

Actions performed:

- Read `GITHUB_PAT_TOKEN` from `/adapt/secrets/m2.env` without printing the token.
- Re-authenticated `gh` for `github.com`; `gh auth status` reports `ADAPT-Chase` active with repository scope.
- Verified API permissions for `TeamADAPT/codex-desktop-linux`: admin, maintain, pull, push, and triage.
- Configured Git HTTPS to use `gh auth git-credential`.
- Verified the repaired credential path with a dry-run push of `working`; no remote ref was changed by the verification.

Files touched:
- `/home/x/.config/gh/hosts.yml`
- `/home/x/.gitconfig`
- `ops/operations_history.md`
- `ops/decisions.log`

**— Codex**

## 2026-07-15 02:24:02 — Codex
Committed the synchronization plan locally, then stopped when the required `origin/working` push could not authenticate.

Actions performed:

- Created local commit `9d310be` (`Plan upstream origin local synchronization — Codex`).
- Attempted `git push origin working`; Git reported `could not read Username for 'https://github.com'`.
- Checked GitHub CLI authentication; the configured `ADAPT-Chase` token is invalid.
- Checked non-interactive SSH authentication without accepting a new host key; GitHub is not present in the user's known-hosts file, so SSH was not used.
- Left the synchronization task in `ops/to_do/`; no source refs or source files were changed.

Files touched:
- `.git/objects/`
- `.git/refs/heads/working`
- `ops/operations_history.md`
- `ops/decisions.log`

**— Codex**

## 2026-07-15 02:22:59 — Codex
Created the upstream-to-origin-to-local synchronization task and approval-gated execution plan after verifying the clean worktree and remote topology.

Actions performed:

- Verified `upstream` is `ilysenko/codex-desktop-linux` and `origin` is the TeamADAPT fork.
- Verified `upstream/main` is `52e9701e3f1be291821cff904b6cd4bdce30998d` while `origin/main` is `fa5baa3896d38c7bfa98eee4875bd973c62bca05`.
- Switched from local `dev/mobile-gpu-route` to the dedicated `working` branch.
- Added the task to `ops/to_do/` and documented the non-force synchronization plan.
- Left source refs and source files unchanged pending user approval.

Files touched:
- `.git/HEAD`
- `plans/sync-upstream-origin-local.md`
- `ops/to_do/sync_upstream_origin_local/task.md`
- `ops/operations_history.md`
- `ops/decisions.log`

**— Codex**

## 2026-07-01 06:40:13 — Codex
Reverified the Codex Desktop Linux install from the live system and reconciled the local branch with `origin/working`.

Actions performed:

- Fetched `origin/working` after the initial push was rejected as non-fast-forward.
- Inspected the remote task history and found the native install task already completed under `ops/completed/install_codex_desktop_linux/`.
- Skipped the duplicate local planning commit and realigned local `working` to track `origin/working`; no force push was used.
- Verified Ubuntu 24.04 live package state with `dpkg-query`.
- Verified installed root-owned paths under `/opt/codex-desktop`, `/usr/bin/codex-desktop`, `/usr/bin/codex-update-manager`, and `/usr/lib/systemd/user/codex-update-manager.service`.
- Verified updater daemon process `/usr/bin/codex-update-manager daemon` is running.
- Verified the user systemd unit is enabled by `~/.config/systemd/user/default.target.wants/codex-update-manager.service`.
- Verified `codex-update-manager status --json` reports installed version `2026.06.26.210745` and Codex CLI status `up_to_date`.
- Noted a pending updater-built package `2026.06.27.030913+91c7139d` whose privileged install was previously dismissed.

Files touched:
- `.git/config`
- `.git/refs/heads/working`
- `ops/operations_history.md`
- `ops/decisions.log`
- `ops/completed/install_codex_desktop_linux/completion_report.md`

## 2026-07-01 03:06:06 — Codex
Created the native Rust host plus wasm64 components refactor plan for repository review.

Actions performed:

- Added a strategic plan at `plans/rust-wasm64-components.md`.
- Updated `implementation_plan.md` with the tactical phase plan and approval gate.
- Left implementation code unchanged pending user approval.

Files touched:
- `plans/rust-wasm64-components.md`
- `implementation_plan.md`
- `ops/operations_history.md`
- `ops/decisions.log`

## 2026-06-26 22:34:01 — Codex
Repaired the remaining Codex Desktop chat visibility issue by registering the restored local history under Desktop project roots instead of modifying the thread database again.

Actions performed:

- Inspected the live Electron UI through a temporary local CDP port on the running `/usr/bin/codex-desktop` launch.
- Verified direct `codex app-server --stdio` `thread/list` returned `13` restored local threads.
- Verified Desktop global Search could see restored history while the sidebar `Chats / No chats` section was projectless.
- Used the app bridge message `electron-update-workspace-root-options` to register `/adapt/projects/rusty_oai/codex-desktop-linux` as a Desktop project root.
- Verified the live UI showed the `Projects` section with `codex-desktop-linux`; user confirmed the chats are visible and working.

Receipts:

- Direct app-server `thread/list`: `13` local threads.
- Live Desktop UI: global Search displayed restored history results.
- Live Desktop UI: project list displayed `codex-desktop-linux`.
- User confirmation: `perfect..it works`.

Files touched:
- `/home/x/.config/Codex/`
- `/home/x/.cache/codex-desktop/manual-launch.stdout`
- `/home/x/.cache/codex-desktop/manual-launch.stderr`
- `/home/x/.cache/codex-desktop/launcher.log`
- `ops/operations_history.md`
- `ops/decisions.log`
- `ops/completed/repair_desktop_project_scope/completion_report.md`

## 2026-06-26 21:23:48 — Codex
Repaired the local Codex Desktop thread catalog after the installed app launched with current/history chats missing.

Actions performed:

- Stopped only the running Codex Desktop app processes under `/opt/codex-desktop`.
- Backed up the Desktop catalog database to `/home/x/.codex/sqlite/backups/codex-desktop-catalog-20260626212215/`.
- Rebuilt `local_thread_catalog` in `/home/x/.codex/sqlite/codex-dev.db` from active rows in `/home/x/.codex/state_5.sqlite`.
- Set `local_thread_catalog_sync_state.initial_build_complete=1`.
- Relaunched `/usr/bin/codex-desktop`.

Receipts:

- Source state DB active threads: `13`.
- Rebuilt Desktop catalog visible local threads: `13`.
- Catalog revision advanced to `1`.
- Running app log shows successful `thread/list` responses after relaunch.
- Running app processes include `/bin/bash /opt/codex-desktop/start.sh`, `python3 /opt/codex-desktop/.codex-linux/webview-server.py 5175 --bind 127.0.0.1`, and `/opt/codex-desktop/electron`.

Files touched:
- `/home/x/.codex/sqlite/codex-dev.db`
- `/home/x/.codex/sqlite/backups/codex-desktop-catalog-20260626212215/codex-dev.db`
- `/home/x/.cache/codex-desktop/manual-launch.stdout`
- `/home/x/.cache/codex-desktop/manual-launch.stderr`
- `/home/x/.cache/codex-desktop/launcher.log`
- `ops/operations_history.md`
- `ops/decisions.log`
- `ops/completed/repair_desktop_thread_catalog/completion_report.md`

## 2026-06-26 21:15:24 — Codex
Launched installed Codex Desktop from `/usr/bin/codex-desktop`.

Command run:

```bash
setsid /usr/bin/codex-desktop >"$HOME/.cache/codex-desktop/manual-launch.stdout" 2>"$HOME/.cache/codex-desktop/manual-launch.stderr" < /dev/null &
```

Receipts:

- Launch command returned PID `55065`.
- Running launcher process: `/bin/bash /opt/codex-desktop/start.sh`.
- Running webview server: `python3 /opt/codex-desktop/.codex-linux/webview-server.py 5175 --bind 127.0.0.1`.
- Running Electron process: `/opt/codex-desktop/electron`.
- Launcher log shows local webview asset HTTP `200` responses and bundled plugin reconciliation completion.

Files touched:
- `/home/x/.cache/codex-desktop/manual-launch.stdout`
- `/home/x/.cache/codex-desktop/manual-launch.stderr`
- `/home/x/.cache/codex-desktop/launcher.log`
- `ops/operations_history.md`
- `ops/decisions.log`

## 2026-06-26 21:09:11 — Codex
Completed native install task and moved its task directory from `ops/in_progress/` to `ops/completed/`.

Files touched:
- `ops/operations_history.md`
- `ops/decisions.log`
- `ops/completed/install_codex_desktop_linux/task.md`
- `ops/completed/install_codex_desktop_linux/completion_report.md`

## 2026-06-26 21:08:50 — Codex
Installed Codex Desktop Linux through the native Ubuntu `.deb` bootstrap path and verified the live installed system.

Command run:

```bash
make bootstrap-native
```

Receipts:

- `dpkg-query`: `codex-desktop 2026.06.26.210745 amd64 install ok installed`
- Installed launcher: `/usr/bin/codex-desktop`
- Installed updater: `/usr/bin/codex-update-manager`
- Installed app root: `/opt/codex-desktop`
- systemd user service: `codex-update-manager.service` loaded, enabled, and active/running
- `codex-update-manager status --json`: status `idle`, installed version `2026.06.26.210745`, CLI status `up_to_date`

Files touched:
- `Codex.dmg`
- `Codex.dmg.metadata`
- `codex-app/`
- `dist/`
- `target/`
- `/opt/codex-desktop`
- `/usr/bin/codex-desktop`
- `/usr/bin/codex-update-manager`
- `/usr/lib/systemd/user/codex-update-manager.service`
- `ops/operations_history.md`
- `ops/decisions.log`
- `ops/completed/install_codex_desktop_linux/completion_report.md`

## 2026-06-26 21:03:13 — Codex
Initialized ops structure and install task tracking for native Codex Desktop Linux install.

Files touched:
- `implementation_plan.md`
- `ops/operations_history.md`
- `ops/decisions.log`
- `ops/in_progress/install_codex_desktop_linux/task.md`
