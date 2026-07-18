# Fix Codex Desktop launch reliability and model picker visibility

## Diagnosis

1. **Launch failures (hard stop)**  
   Launcher selected `~/.local/bin/codex` (first on PATH). That tree was installed
   under umask `0002`, so files/ancestors were group-writable. The execution-free
   CLI trust check rejects those paths and exits before Electron starts. Trusted
   CLIs already exist at `/usr/local/bin/codex` and `/usr/bin/codex`.

2. **Models appear incomplete**  
   Account auth is ChatGPT. Upstream keeps Statsig/account model rollouts; this
   wrapper does not unlock them. The compact Power slider also hides Sol/Terra/Luna
   behind a nested Model control. `ui-tweaks` / `modelPicker.showModelsByDefault`
   expands the full model list in the composer picker.

## Plan

1. Runtime: strip group/world-write from the user-local npm Codex tree (done).
2. Launcher: on auto-discovered CLI trust failure, fall through to the next
   trusted candidate; keep hard-fail for explicit `CODEX_CLI_PATH`.
3. Features (local gitignored): enable `ui-tweaks` with
   `modelPicker.showModelsByDefault`.
4. Rebuild/reinstall so template + feature patches land in `/opt/codex-desktop`.
5. Live receipt: systemd launch, webview 200, trust verified, model/list ok.

## Non-goals

- Unlock ChatGPT-account-only model rollouts OpenAI has not enabled for the account.
- Enable `api-key-model-visibility` while auth remains `chatgpt` (that feature is
  for API-key hosts only).
