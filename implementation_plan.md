# Implementation Plan

## 2026-06-26 21:03:13 — Codex
Plan: install `codex-desktop-linux` on this Ubuntu 24.04 host using the repository-documented native bootstrap path.

Steps:

1. Create the required ops trail under `ops/` and track this install task under `ops/in_progress/install_codex_desktop_linux/`.
2. Run `make bootstrap-native` to install host dependencies, build a fresh `codex-app/`, package a `.deb`, and install it with the Makefile install target.
3. Verify the installed package and updater service from the live system using package metadata, command paths, and systemd/user-service status where available.
4. Write `ops/in_progress/install_codex_desktop_linux/completion_report.md`, move the task to `ops/completed/`, and log completion.

Approval basis: direct user instruction received on 2026-06-26 to install this repository.

