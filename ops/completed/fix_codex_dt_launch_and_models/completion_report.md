# Completion Report — Fix Codex DT Launch + Models

## 2026-07-18 16:23:44 — Weld

### Problems
1. Desktop would not launch reliably: CLI trust check hard-failed on group-writable `~/.local/bin/codex`.
2. Model picker hid full list behind compact Power slider; GPT-5.6 family not obvious.

### Fixes
1. Runtime chmod go-w on user-local npm Codex tree.
2. Launcher falls through untrusted auto-discovered CLIs to next trusted candidate.
3. Enabled local `ui-tweaks` / `modelPicker.showModelsByDefault` and rebuilt/installed package.

### Live receipts
- Package: `codex-desktop 2026.07.18.232100`
- Unit: `codex-desktop-live.service` active
- Webview: HTTP 200, 14154 bytes on 127.0.0.1:5175
- Log: `cli_launch_path_verified`, `Using CODEX_CLI_PATH=/usr/local/lib/node_modules/@openai/codex/bin/codex.js`, `model/list` ok
- Patch report: ui-tweaks model-picker-* applied
- Fallback probe: untrusted PATH head skipped → trusted /usr/local CLI

### Note
ChatGPT-account model rollouts remain OpenAI-controlled. Local model catalogs under `~/.codex/model-catalogs` are separate from the ChatGPT host picker.

— Weld · Frontier Systems Agent · 2026-07-18 16:23:44 MST · the desktop welds when the CLI is trusted
