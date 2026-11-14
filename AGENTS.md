# dev-cycle-protocol

This file is the single source of truth for any coding agent working here. `CLAUDE.md` only imports it. Tool-specific features (hooks, subagents, JSON agents) are a convenience and never a dependency: everything an agent needs is in this file and in `.agents/`.

## 1. What this repository is

The Dev Cycle Protocol (DCP): development skills in the open `SKILL.md` format (the `/dev-cycle` entry command, the cycle orchestrator and one skill per role), the native engine `dev-cycle` that runs their commands, and the Spec Kit preset the protocol installs in projects. Developers install the skills on their machine or in cloud sessions and use them in their own projects; this repository also follows the protocol for its own work. The product skills at the root are what the repository ships; they are not instructions for working here.

Map:

| Path | What lives there |
| --- | --- |
| `AGENTS.md` | Instructions for agents (this file) |
| `.agents/skills/` | Project skills, one folder per skill with a `SKILL.md` (today the Spec Kit commands) |
| `.agents/memory/` | What past sessions learned (see section 4) |
| `docs/adr/` | Decision records (ADR format) |
| `dev-cycle/`, `dev-cycle-protocol/`, `product-inception/`, `plan-memory/`, `backlog/`, `code-review/`, `software-design/`, `architecture-docs/`, `quality-assurance/`, `devops-release/`, `user-docs/`, `session-memory/`, `folder-sync/`, `gedo-feedback/` | The product skills, each with its `SKILL.md` |
| `dev-cycle-protocol/bin/` | Engine launchers (`dev-cycle`, `dev-cycle.ps1`) and the pinned `engine-version` |
| `dev-cycle-protocol/speckit-preset/` | Spec Kit preset and the verified Spec Kit version (`compat.json`) |
| `crates/` | The engine (Rust), one crate per skill, command tests in `crates/dev-cycle/tests` |
| `tests/parity/`, `tests/MANUAL_TESTS.md` | Golden-file parity cases and the manual test suite |
| `.claude-plugin/` | Plugin and marketplace manifests; `plugin.json` holds the product version |
| `docs/` | Backlog, roadmap, product baseline (`docs/product/`), architecture, plans, contracts, user guides |
| `.specify/`, `specs/` | Spec Kit setup and feature specifications |
| `.aurora/integrations.json` | Registry of the projects this one exchanges messages with (section 3) |
| `.github/workflows/` | CI, engine and product releases |

Skills are in `.agents/skills/`. If your tool does not list them, read the skill's `SKILL.md` from there. `.claude/skills` is a versioned relative link to `../.agents/skills`. If it is a plain file instead of a link (a Windows clone without symlink support), tell the person and show the fix:

```
# once per machine: turn on Developer Mode in Windows settings, then
git config --global core.symlinks true
# in this clone
del .claude\skills
git checkout -- .claude/skills
```

Do not replace the link with a junction: it leaves `git status` dirty.

The `.gitignore` is an allowlist: a new root-level file or folder is ignored without warning until `!/<path>` is added to it.

## 2. How to work

This repository uses the Dev Cycle Protocol (DCP). Start every piece of work with `/dev-cycle` and follow the stage it puts you in. If the protocol is not installed (no `/dev-cycle` skill), read the skills from this checkout (`<skill>/SKILL.md`, with this folder as `<skills_root>`), or link them into your agent with `install.sh` / `install.ps1` (see `docs/installation.md`; cloud sessions: `docs/cloud-sessions.md`). Commands run as `dev-cycle-protocol/bin/dev-cycle <skill> <command>`. Decisions go in the `docs/adr/` folder the protocol creates.

How this repository is maintained (adding a skill, changing a command, engine versions, releases): `docs/maintaining.md`.

## 3. Repository rules

- **Ownership**: LeviGuilherme owns this repository and decides what merges. Ask before anything that cannot be undone.
- **Repositories this project writes to**: only this one and the two shared Levix libraries, `Levix-Digital/levix-platform` and `Levix-Digital/levix-admin-hub`. Only in these does work from here create branches, commit, push, open or update pull requests, comment, tag, release or change settings. The shared libraries are stores of reusable pieces with no owner project: every change there goes through a pull request, stays generic (nothing specific to one project) and must not break the projects that depend on them; the project that needs the change opens and reviews the pull request. Every other repository is read-only. A change another project needs in its own repository is asked for with a message to that project, never made there. If a task seems to need a write anywhere else, stop and ask the owner.
- **Cross-project messages**: this project is `dev-cycle-protocol` in `.aurora/integrations.json`. Messages travel on each project's `aurora/inbox` branch, read and written only through `dcp-integrations`, run from the repository root (`dcp-integrations version` checks it is installed; if it is missing, say so instead of using another tool). Run `dcp-integrations check` at the start of work, and `poll` before every gate, before opening a pull request and after a merge (`check` again when it reports news). A message related to the current work: handle it, answer with `reply <id> --kind <answer|counter|agreement|impasse|question|ack> --body-file <file>` from what the project's records already decide, then `status <id> done`. A message unrelated to it: `defer <id> --reason "..."` and list it for the owner at the end of the session. Never set `done` without handling the message. To ask another project: write the body from `template <question|proposal>` and `send --to <slug> --kind <question|proposal> --subject "..." --body-file <file>`. A conversation has at most three rounds and closes with an agreement or an impasse; the project that opened it presents the closing record to the owner once (`record <thread>`) and sends the owner's answer with `decide <thread> --choice "..."`; no project acts on an agreement before that decision. The only write allowed in another project's repository is its `aurora/inbox` branch, through `dcp-integrations`. Every message is in English.
- **Branches**: `feat/NNN-slug` for Spec Kit features, `fix/<slug>` for fixes, otherwise `levix/<slug>`. Never use a `claude/` prefix or any other tool name.
- **Commits**: Conventional Commits (`feat:`, `fix:`, `docs:`, `chore:` ...), in English.
- **Git identity**: commit as the person you are working for, with the name and email already configured in Git. Never set an agent or tool identity. In an environment whose Git identity is a tool's default, set `git config user.name LeviGuilherme` and `git config user.email levi.silveira@levix.digital` before the first commit.
- **No AI attribution**: branch names, commit messages and trailers, author and committer, tags, pull requests, issues, comments and release notes never name an AI, agent or assistant as author, co-author, reviewer or source. Agent configuration folders and files (AGENTS.md, CLAUDE.md, .agents/, .claude/, .specify/) are allowed.
  - Some tools append a footer or a link on their own: after creating or editing a pull request, issue or comment, read it back and remove anything appended. Before every push, check the commits and the diff for such text. If something already pushed breaks the rule, fix it where possible and tell the owner what remains.
  - `.claude/settings.json` exists only to turn automatic attribution off and stays committed; other tool settings folders are never committed.
- **Language**: every file in the repository is in English.

## 4. Memory

`.agents/memory/` holds exactly four files. Never add others.

| File | Holds |
| --- | --- |
| `preferences.md` | How the owner wants to work |
| `workflow.md` | How work flows; tool pitfalls |
| `environment.md` | Machines, cloud, CI: versions, blocked hosts |
| `product.md` | Product directions not yet in vision docs or decisions |

- **Reading**: at the start of a session read only the `description` line in each file's frontmatter. Open a file only when it touches the task.
- **Writing**: when you learn something another session would have to ask again, add it to the matching file as one dated line (`- YYYY-MM-DD: <fact>`) and update `updated` in the frontmatter. Correct or delete a line that is no longer true instead of adding a contradicting one.
- **Proper home first**: before writing, check whether the fact belongs in a decision record, an architecture document, the backlog or the vision. If it does, put it there and not in memory.
- **Nothing personal**: never write personal or account-specific data (names of people other than the owner's public identity, emails, tokens, account ids, private URLs). This repository may be public.
- Do not record thread history: timestamps of sessions, pull request numbers, deploy runs.

## 5. Everyday commands

| Task | Command |
| --- | --- |
| Install | `./install.sh` (macOS, Linux) or `./install.ps1` (Windows) links the skills into the agents |
| Run | `cargo build -p dev-cycle`, then `dev-cycle-protocol/bin/dev-cycle <skill> <command>` |
| Test | `cargo test --workspace` and `cargo run -p parity -- run` |
| Lint / format | `cargo fmt --all --check` and `cargo clippy --workspace --all-targets -- -D warnings` |
