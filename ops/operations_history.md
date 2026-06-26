# Operations History

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
