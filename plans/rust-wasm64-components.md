# Native Rust Host + wasm64 Components Refactor Plan

## 2026-07-01 03:06:06 — Codex

Status: pending user review and approval. No implementation code changes are authorized by this plan until approval is recorded.

## Decision

Refactor this repository toward a native Rust host plus wasm64 component architecture.

The native Rust host owns ambient authority and platform effects. wasm64 components own deterministic build logic, feature logic, patch planning, and package payload transforms.

This is stronger than a Rust-only rewrite because optional Linux integrations become typed capability modules instead of trusted shell hooks. It is stronger than making wasm64 own every outer effect because privileged host integration remains debuggable, receipted, and aligned with Linux system boundaries.

## Target Architecture

```text
install.sh / existing package entrypoints
        |
        v
codex-build-host          native Rust CLI and library
        |
        +-- host capabilities
        |   +-- filesystem, modes, symlinks
        |   +-- process execution
        |   +-- network download and hash verification
        |   +-- archive, DMG, ASAR, and payload extraction
        |   +-- distro and desktop detection
        |   +-- package-manager and package-format operations
        |   +-- systemd user-service operations
        |   +-- pkexec/polkit escalation bridge
        |
        v
wasm64 components
        +-- install graph
        +-- Linux feature validation and staging graph
        +-- ASAR and app patch graph
        +-- package payload graph
        +-- optional feature components
```

Components return typed intent. The host applies or rejects intent, records receipts, and owns rollback metadata.

Example intent:

```text
copy source resource to app path with mode 0755
add runtime hook owned by feature id
patch app bundle target with declared matcher
run native command with bounded environment
emit package payload entry with owner, group, mode, and hash
```

## Design Rules

- Keep `install.sh` and package shell entrypoints as compatibility wrappers until live parity is proven.
- Keep generated output out of durable edits. Change source templates, builders, descriptors, or components.
- Keep optional features disabled by default.
- Keep feature-specific behavior in `linux-features/` or feature components.
- Keep native-package-only behavior in package-specific host adapters.
- Prefer plan/apply execution: compute first, mutate second, receipt every mutation.
- Require live system receipts for acceptance: package metadata, file state, service state, app launch, updater status, and HTTP/app responses where applicable.
- Treat shell and Node migration as staged compatibility work, not a one-shot rewrite.

## Interface Model

Define stable component interfaces before porting behavior:

- `codex:build/target-context`: distro, package format, desktop, architecture, install identity.
- `codex:build/features`: discover, validate, resolve dependencies, resolve conflicts.
- `codex:build/resources`: declare resource staging, ownership, mode, and cleanup behavior.
- `codex:build/patches`: declare patch targets, matchers, transforms, required/fail-soft policy, and drift metadata.
- `codex:build/packages`: declare package payload entries and format-specific metadata.
- `codex:build/receipts`: emit operation events, hashes, warnings, decisions, and touched paths.
- `codex:build/commands`: request bounded native command execution through host policy.

These interfaces can map to the internal wasm64/custom runtime directly. They should remain host-owned contracts, not ambient filesystem/process access from arbitrary modules.

## Phase 0: Inventory And Approval

Goal: create a complete migration map before code changes.

Work:

1. Inventory `install.sh`, `scripts/lib/*.sh`, package builders, `linux-features.js`, patch registry JS, and updater rebuild paths.
2. Classify each side effect as host capability, component logic, compatibility wrapper, or generated output.
3. Document the initial component ABI and host capability list.
4. Record approval before Phase 1 begins.

Acceptance:

- Migration map committed.
- Approval recorded in `implementation_plan.md` and `ops/decisions.log`.
- No behavior-changing code modified.

## Phase 1: Rust Host Foundation

Goal: add the host without replacing current behavior.

Work:

1. Add workspace crates for shared build types and host CLI, expected names:
   - `codex-build-core`
   - `codex-build-host`
2. Implement initial CLI subcommands:
   - `inspect`
   - `plan install`
   - `plan package`
   - `apply`
   - `verify`
3. Implement structured operation events and receipt output.
4. Keep existing shell scripts as the operational path.

Acceptance:

- `cargo check` passes for new crates and existing workspace crates.
- Host can emit a no-op plan and receipt from the current repository state.
- Existing shell validation remains green.

## Phase 2: Typed Host Capabilities

Goal: make every mutating operation explicit and receipted.

Work:

1. Add host capability adapters for filesystem operations, process execution, network downloads, archive extraction, distro detection, package metadata, and systemd user-service inspection.
2. Add an operation journal format with touched paths, hashes, command receipts, and warnings.
3. Add dry-plan rendering without filesystem mutation.
4. Add live temp-root and installed-system verification modes.

Acceptance:

- Host can inspect live package/updater state without changing it.
- Host can stage controlled filesystem operations into a temp app root and emit a receipt.
- Receipt schema includes all touched paths and command results.

## Phase 3: Feature Framework Migration

Goal: move Linux feature discovery and validation into the typed host/component boundary.

Work:

1. Port feature manifest parsing and validation from `scripts/lib/linux-features.js` to Rust types.
2. Preserve current manifest rules: no `defaultEnabled: true`, required `README.md`, dependency/conflict validation, safe resource targets, quoted octal modes, and local feature namespace checks.
3. Add optional wasm64 component declaration support to feature manifests.
4. Keep existing JS feature tests and add Rust parity tests.
5. Keep legacy `patch.js`, `patches.js`, and `stage.sh` paths working through compatibility adapters.

Acceptance:

- Current feature config behavior is unchanged.
- Existing `linux-features/*/test.js` behavior remains valid.
- Host emits structured feature validation receipts.

## Phase 4: Package Staging Migration

Goal: move shared package staging out of shell while preserving each package format.

Work:

1. Port shared `scripts/lib/package-common.sh` behavior into Rust host services.
2. Model package payload entries with path, owner, group, mode, source hash, and owning feature/core component.
3. Keep `build-deb.sh`, `build-rpm.sh`, `build-pacman.sh`, and `build-appimage.sh` as wrappers.
4. Add package-format adapters only where format-specific behavior is required.

Acceptance:

- `.deb`, `.rpm`, pacman, and AppImage payloads are generated from the same typed payload graph.
- Package inspection commands verify expected metadata and file modes.
- Updater bundle staging remains intact.

## Phase 5: Install Graph Migration

Goal: replace the installer orchestration path with Rust host plus wasm64 build graph components.

Work:

1. Port install sequencing from `install.sh` into an install graph component plus Rust host apply logic.
2. Preserve current environment overrides and CLI behavior.
3. Keep the shell entrypoint as a thin wrapper.
4. Generate `codex-app/start.sh` only from `launcher/start.sh.template` and install-time identity data.

Acceptance:

- `./install.sh ./Codex.dmg` still works through the wrapper.
- Live app staging receipts include DMG source, Electron version, Node runtime, native module rebuilds, webview assets, bundled plugins, enabled features, and launcher generation.
- Existing shell syntax checks remain green.

## Phase 6: Patch Graph Migration

Goal: make patching structured while reducing Node-specific patch authority.

Work:

1. Wrap current Node patcher output into typed patch receipts.
2. Move patch descriptor metadata into host/component-readable structures.
3. Port low-risk patch transforms to wasm64 components first.
4. Keep high-risk ASAR and upstream-drift-sensitive transforms on the compatibility path until parity is proven.

Acceptance:

- Patch reports preserve required/fail-soft behavior.
- Upstream drift failures include target, matcher, expected context, and owning descriptor.
- Live built app launches after patch migration.

## Phase 7: Updater Integration

Goal: make local auto-update rebuilds use the same host and component graph as fresh installs.

Work:

1. Link or invoke `codex-build-host` from `codex-update-manager`.
2. Bundle host binaries, wasm64 components, feature config, and local feature root metadata in the update-builder bundle.
3. Preserve rollback state and privileged install boundaries.
4. Add receipts for update check, rebuild, package install, rollback, and service state.

Acceptance:

- `codex-update-manager status --json` still reports live state.
- Rebuild candidates use the same component versions as native package builds.
- Failed privileged installs remain failed until explicit retry or newer rebuild.

## Phase 8: Decommission Legacy Paths

Goal: remove compatibility code only after live receipts prove parity.

Work:

1. Mark legacy shell and JS modules as compatibility-only.
2. Delete or freeze compatibility paths after equivalent Rust/wasm64 paths pass live install and package receipts.
3. Update README, architecture docs, build docs, Linux feature docs, and updater docs.

Acceptance:

- Public commands still work.
- Native packages still install and manage the updater through systemd.
- Optional features remain disabled by default and keep local/private feature support.
- Docs match the real operational state.

## Validation Matrix

Minimum validation for implementation phases:

```bash
bash -n install.sh
bash -n scripts/lib/*.sh
bash -n launcher/start.sh.template
bash -n scripts/build-deb.sh
bash -n scripts/build-rpm.sh
bash -n scripts/build-pacman.sh
bash -n scripts/build-appimage.sh
node --test scripts/patch-linux-window-ui.test.js
node --test linux-features/*/test.js
cargo check --workspace
cargo test --workspace
./scripts/build-deb.sh
dpkg-deb -I dist/codex-desktop_*.deb
dpkg-deb -c dist/codex-desktop_*.deb
systemctl --user status codex-update-manager.service
codex-update-manager status --json
```

Format-specific work must also run the matching package builder and metadata inspection for RPM, pacman, and AppImage.

Launcher-affecting work must verify the generated launcher and live app launch from the installed wrapper.

## Risk Register

- wasm64 runtime ABI drift: pin component interface versions and reject incompatible components before apply.
- privilege boundary ambiguity: keep pkexec and package-manager actions host-owned and fully receipted.
- shell parity gaps: keep wrappers until live package/install receipts prove replacement behavior.
- ASAR patch drift: migrate patch classes gradually and preserve structured failure reports.
- feature compatibility gaps: keep legacy feature hooks available until each feature has a typed replacement path.
- package mode/ownership regressions: model payload metadata explicitly and inspect built artifacts.
- updater rebuild mismatch: bundle exact host/component versions and record them in rebuild reports.

## Approval Gate

Implementation may begin only after the user approves this plan or a superseding version.

Approval record required:

- `implementation_plan.md`
- `ops/decisions.log`

