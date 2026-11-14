---
name: environment
description: Machines, cloud and CI: versions, blocked hosts, setup quirks. Read when a command, install or network call fails or behaves differently between machines.
updated: 2026-11-14
---

- 2026-11-14: Cloud sessions have no gh CLI; use the platform API for pull requests, reviews and comments.
- 2026-11-14: In cloud sessions, deleting a remote branch through the session's Git proxy is refused (HTTP 403); leftover branches are deleted by the owner on GitHub.
- 2026-11-14: Spec Kit commands that install or upgrade (speckit_setup init, check, upgrade, specify integration ...) need uv and network access to github.com; run specify as `uvx --from git+https://github.com/github/spec-kit.git@<version> specify ...` with the version pinned in .dev-cycle.json.
- 2026-11-14: `specify integration switch` drops the executable bit on .specify/scripts/bash/*.sh; restore it (`chmod +x`, or `git update-index --chmod=+x`) before committing.
