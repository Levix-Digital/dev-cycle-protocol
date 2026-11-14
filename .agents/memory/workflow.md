---
name: workflow
description: How work flows in this repository and the tool pitfalls already hit. Read before branching, committing, opening a pull request or releasing.
updated: 2026-11-14
---

- 2026-11-14: CI runs only on pull requests into main; a branch pushed without a pull request gets no checks.
- 2026-11-14: Any change to a Cargo.toml needs the engine version bumped (Cargo.toml and dev-cycle-protocol/bin/engine-version); the CI lint refuses it otherwise.
- 2026-11-14: The macOS CI job sometimes gets no runner and hangs; run the job again.
- 2026-11-14: Command output on Windows ends lines with \r\n; tests that compare output must normalize it.
- 2026-11-14: The parity golden files embed the verified Spec Kit version: when compat.json moves to a new version, record the affected golden files again in the same change.
- 2026-11-14: Gate 3 checks the latest plan with status active or in_progress; an older open plan can make it pass or fail unexpectedly.
- 2026-11-14: Where the platform CLI (gh, glab) is not installed, pr_sync cannot post the review summary; post the [review] comment through the platform API and read it back for appended footers.
- 2026-11-14: docs/product/PITCH.md is English only, about one page, keeps its section order, describes only released behaviour as "today" and the roadmap as "coming soon" without dates, and copies the one-liner from the Positioning section of VISION.md; update it in the same pull request that changes the vision or a release scope.
