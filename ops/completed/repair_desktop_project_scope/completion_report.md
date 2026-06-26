# Repair Desktop Project Scope Completion Report

## 2026-06-26 22:34:01 — Codex
Completed the remaining chat visibility repair for the installed Codex Desktop Linux app.

Result:

- The local thread data was already present after the catalog repair.
- Direct `codex app-server --stdio` `thread/list` returned `13` local threads.
- Desktop global Search displayed restored history results.
- The sidebar `Chats / No chats` label was confirmed to be the projectless section, not missing data.
- `/adapt/projects/rusty_oai/codex-desktop-linux` was registered as a Desktop project root through the app bridge.
- The live UI showed the `Projects` section with `codex-desktop-linux`.
- User confirmed the repaired UI works.

Files touched:

- `/home/x/.config/Codex/`
- `/home/x/.cache/codex-desktop/manual-launch.stdout`
- `/home/x/.cache/codex-desktop/manual-launch.stderr`
- `/home/x/.cache/codex-desktop/launcher.log`
- `ops/operations_history.md`
- `ops/decisions.log`
- `ops/completed/repair_desktop_project_scope/completion_report.md`

— Codex
