# Update ChatGPT Desktop For Work Mode

## Status

Complete. Approved by the user's 2026-07-15 update instructions and executed on
2026-07-15.

## Objective

Rebuild and install the current Linux package from the newest accepted OpenAI
desktop DMG so the installed application includes the ChatGPT Work/Codex mode
surface available to the signed-in account.

## Plan

1. Record repository, package, process, disk, and privilege preflight receipts.
2. Confirm the installed application is stopped.
3. Run `PACKAGE_WITH_UPDATER=0 make update-native` from the trusted checkout.
4. Verify the installed package and build provenance against the generated package.
5. Verify the installed ASAR contains the Work/Codex selector and expected setting schema.
6. Launch the installed application and collect live process and launcher-log receipts.
7. Record the account-rollout boundary without forcing `conversationDetailMode`.

## Constraints

- Use native system tools and packages; no Docker or Python virtual environment.
- Do not enable optional Linux features.
- Do not modify the user's current Work/Codex setting.
- Do not bypass upstream DMG acceptance checks.
- Build without the local update manager as requested.

**— Codex**
