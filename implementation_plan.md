# Implementation Plan

## 2026-07-01 03:06:06 — Codex
Plan: refactor this repository toward a native Rust host plus wasm64 components for install, feature, patch, package, and updater rebuild orchestration.

Status: pending user review and approval. No implementation code changes are authorized until approval is recorded.

Primary plan document:

- `plans/rust-wasm64-components.md`

Architecture decision:

- Native Rust host owns filesystem mutation, process execution, network/hash verification, archive extraction, distro detection, package-manager/systemd/pkexec integration, receipts, and rollback metadata.
- wasm64 components own deterministic install graph logic, Linux feature validation/staging, patch planning, package payload transforms, and optional feature behavior.
- Existing shell and Node paths remain compatibility wrappers until live package/install receipts prove parity.

Execution phases:

1. Inventory current shell/JS/updater side effects and record the component ABI.
2. Add Rust host foundation without replacing current behavior.
3. Add typed host capabilities and receipt journals.
4. Migrate Linux feature discovery/validation to Rust/component contracts.
5. Migrate shared package staging to typed payload graphs.
6. Migrate install orchestration behind compatibility wrappers.
7. Migrate patch graph behavior gradually, preserving Node compatibility for high-risk ASAR transforms.
8. Integrate updater rebuilds with the same host/component graph.
9. Decommission legacy paths only after live receipts prove parity.

Approval required before Phase 1 implementation.

## 2026-06-26 21:03:13 — Codex
Plan: install `codex-desktop-linux` on this Ubuntu 24.04 host using the repository-documented native bootstrap path.

Steps:

1. Create the required ops trail under `ops/` and track this install task under `ops/in_progress/install_codex_desktop_linux/`.
2. Run `make bootstrap-native` to install host dependencies, build a fresh `codex-app/`, package a `.deb`, and install it with the Makefile install target.
3. Verify the installed package and updater service from the live system using package metadata, command paths, and systemd/user-service status where available.
4. Write `ops/in_progress/install_codex_desktop_linux/completion_report.md`, move the task to `ops/completed/`, and log completion.

Approval basis: direct user instruction received on 2026-06-26 to install this repository.
