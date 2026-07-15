# ChatGPT Work Desktop Update Report

## 2026-07-15 03:48:22 — Codex

## Result

The approved update completed successfully and the updated application is
running from the installed Debian package.

## Build And Acceptance

- Command: `PACKAGE_WITH_UPDATER=0 make update-native`
- Git pull: already current on trusted `working`.
- OpenAI app version: `26.707.72221`.
- Electron version: `42.1.0`.
- DMG SHA256: `40e34814e74e30943c209ebd4da94cd4de3581a52c5bffbe2bcf2e488d6361c6`.
- Upstream acceptance verdict: `accepted`.
- Acceptance blockers: none.
- Acceptance warnings: none.
- Required core patches: 13 applied, 4 already applied.
- Existing enabled Linux features retained: `remote-control-ui`, `remote-mobile-control`.
- No additional Linux feature was enabled.

The normal 7z warning about DMG symlink extraction occurred, but `ChatGPT.app`
was found and the accepted build continued as documented.

## Package Installation

- Previous package: `2026.06.26.210745`.
- Installed package: `2026.07.15.104248`.
- Architecture: `amd64`.
- Package status: `install ok installed`.
- Package SHA256: `e674f0d151ff17ab9c46b5c8d5d8decea575d587e524ad5ad2e286ef871b0d82`.
- Package profile: no updater, as requested.

Verified absent after installation:

- `/usr/bin/codex-update-manager`
- `/usr/lib/systemd/user/codex-update-manager.service`
- `/opt/codex-desktop/update-builder`

## Work Payload

The installed web assets contain the `ChatGPT Work` / `Codex` product-mode
selector and the `conversationDetailMode` setting schema. The user setting was
preserved without modification:

```toml
[desktop]
conversationDetailMode = "STEPS_COMMANDS"
```

The application therefore opens in Codex mode. Work remains selectable from the
top-left control when the signed-in account and workspace receive the rollout.

## CLI Repair

The first live launch correctly failed the new CLI trust check because the
existing user-local target was group-writable:

`/home/x/.local/lib/node_modules/@openai/codex/bin/codex.js`

The CLI was reinstalled with `sudo` through npm's verified package channel at
version `0.144.4` under root-owned `/usr/local`. The user command symlink now
resolves to:

`/usr/local/lib/node_modules/@openai/codex/bin/codex.js`

All resolved path components are root-owned and non-group-writable. The launcher's
execution-free trust helper accepts the repaired path.

## Live Receipt

The installed application was launched through a transient user systemd unit for
observable lifecycle state:

- Unit: `codex-desktop-live.service`.
- State: active/running.
- Launcher, webview server, Electron main, GPU, network, and renderer processes: running.
- Installed crash metadata version: `26.707.72221`.
- Webview response: HTTP 200, 14,154 bytes, explicit no-cache headers.
- Primary renderer: mounted and visible.
- App-server account, config, model, plugin, and thread calls: responding.
- Browser availability: resolved `available=true` on Linux.

Current-run logs include nonfatal upstream/Linux diagnostics: a transient GPU
context failure, a systemd D-Bus method mismatch, repeated stale Deep Research
icon lookups, and destroyed-window race warnings. None terminated the renderer,
webview, app server, or Electron process.

Machine-readable live evidence is recorded in `live_receipt.md`.

**— Codex**
