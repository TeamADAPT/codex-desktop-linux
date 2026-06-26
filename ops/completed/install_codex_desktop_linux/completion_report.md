# Completion Report

## 2026-06-26 21:08:50 — Codex
Result: Codex Desktop Linux installed successfully on Ubuntu 24.04 through the documented native package flow.

Command run:

```bash
make bootstrap-native
```

Installed package:

```text
codex-desktop 2026.06.26.210745 amd64 install ok installed
```

Live-system receipts:

- `/usr/bin/codex-desktop` exists and is executable.
- `/usr/bin/codex-update-manager` exists and is executable.
- `/opt/codex-desktop` exists and is root-owned.
- `/usr/lib/systemd/user/codex-update-manager.service` exists.
- `codex-update-manager.service` is loaded, enabled, and active/running under `systemd --user`.
- `codex-update-manager status --json` reports `status: "idle"` and `cli_status: "up_to_date"`.

Non-blocking build warnings observed:

- 7z reported DMG symlink extraction warnings, but the app bundle was found and the installer continued.
- Optional `keybinds-settings` ASAR patch skipped because the JSX runtime asset was not found.
- Some optional Browser Use Linux profile patch targets were missing in upstream resources.

Generated or system-installed artifacts:

- `Codex.dmg`
- `Codex.dmg.metadata`
- `codex-app/`
- `dist/codex-desktop_2026.06.26.210745_amd64.deb`
- `target/`
- `/opt/codex-desktop`
- `/usr/bin/codex-desktop`
- `/usr/bin/codex-update-manager`
- `/usr/lib/systemd/user/codex-update-manager.service`

