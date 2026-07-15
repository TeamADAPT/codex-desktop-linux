# ChatGPT Work Mode Review

## 2026-07-15 03:15:50 — Codex

## Findings

### 1. ChatGPT Work is a real upstream product mode

OpenAI's July 9, 2026 release notes introduce ChatGPT Work for longer research,
analysis, connected-app work, and finished deliverables. The official desktop
documentation says the new application combines Chat, Work, and Codex and that
eligible users switch between Work and Codex from the top-left mode selector.

Availability is account- and workspace-gated. OpenAI states that Work is being
rolled out gradually to eligible accounts; absence of the selector is not proof
of a Linux wrapper defect.

Sources:

- <https://help.openai.com/en/articles/6825453-chatgpt-app-features>
- <https://help.openai.com/en/articles/20001275/>
- <https://help.openai.com/en/articles/20001276-moving-to-the-new-chatgpt-desktop-app>

### 2. The latest official desktop payload contains the Work/Codex selector

The official DMG pinned by this repository was downloaded and inspected:

- App version: `26.707.72221`
- Bundle version: `5307`
- Display name: `ChatGPT`
- Bundle identifier: `com.openai.codex`
- DMG SHA256: `40e34814e74e30943c209ebd4da94cd4de3581a52c5bffbe2bcf2e488d6361c6`

The packed web assets contain:

- A `ChatGPT Work` / `Codex` top-left mode selector.
- Work description: `Create, learn, and explore`.
- Codex description: `Build, debug, and ship`.
- A migration announcement describing Work beyond code.
- The `conversationDetailMode` application setting.

Relevant paths inside `app.asar`:

- `webview/assets/app-initial~app-main~page-Cmd9LUYY.js`
- `webview/assets/general-settings-C0l3c9YI.js`
- `webview/assets/app-initial~app-main~hotkey-window-new-thread-page~hotkey-window-home-page~composer-utility-bar-D9zyQF1n.js`

### 3. There is a local setting, but it does not grant rollout eligibility

The setting is:

```toml
[desktop]
conversationDetailMode = "STEPS_COMMANDS"
```

Supported payload values are:

- `STEPS_PROSE`: Work / everyday mode.
- `STEPS_COMMANDS`: Codex / coding mode and the default.
- `STEPS_EXECUTION`: accepted legacy detail value and normalized to coding behavior.

In the latest payload, the Work/Codex product-mode control reads the same detail
state. Static control flow maps `STEPS_PROSE` to Work and coding detail to Codex.
The redesigned selector is guarded by rollout gate `824038554`. When the gate is
off, the older General Settings Work-mode card remains in the payload; when it is
on, that card is hidden and the top-left selector is rendered.

Changing `conversationDetailMode` can select the local presentation mode, but it
does not enable the server rollout, eligible plan, workspace policy, connected
services, or backend role required for the complete ChatGPT Work experience.

### 4. The live Linux installation is still on the prior desktop payload

Live receipt:

- Package: `codex-desktop 2026.06.26.210745`, installed.
- Upstream app version: `26.623.31921`.
- Electron process, launcher, and local webview server: running.
- Current setting: `[desktop] conversationDetailMode = "STEPS_COMMANDS"`.
- Current effective local selection: Codex / coding.

No runtime setting was changed during this review.

### 5. The wrapper changelog does not announce this upstream release

`CHANGELOG.md` on `upstream/main` contains no Work-mode, ChatGPT Work, everyday
work, or knowledge-work entry. The wrapper's latest commit is `79d7303`, which
only adjusts trusted Nix-store CLI paths and tests. It is unrelated to Work.

The wrapper already pins the new upstream application version in `flake.nix`:

```nix
codexVersion = "26.707.72221";
```

The product announcement therefore exists in OpenAI's ChatGPT release notes,
not the Linux wrapper's changelog.

## Conclusion

The new Work mode is present in the latest upstream desktop application. The
live Linux install is one upstream app generation behind and is currently set
to Codex/coding. Updating the Linux package is necessary to obtain the new
desktop payload, but account rollout eligibility still controls whether the new
top-left Work selector and full Work surface become available.

**— Codex**
