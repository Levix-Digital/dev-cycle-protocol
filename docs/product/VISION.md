# Dev Cycle Protocol

A development cycle for coding agents: an engine that conducts the agents phase by phase and deterministic checks that take a request from idea to verified release, with the user deciding at every gate. It works with the main coding agents, on a local machine or in a cloud session, and grows into coordinated parallel agents when the project uses Gedo. Until v3.0.0 the cycle runs as `SKILL.md` skills that the agent follows; from v3.0.0 the engine conducts the agents (ADR-0008).

## Problem

Coding agents made writing code several times faster, and the bottleneck moved to everything around the code:

- **Collisions**: several agents and developers editing the same repository break each other's signatures and fight over the same files and lockfiles.
- **Architectural drift**: agents without deterministic governance cross layers, duplicate abstractions and leave the technical docs and decisions outdated within weeks.
- **Review fatigue**: agent pull requests mix refactoring, formatting and new logic in diffs too large to review safely.
- **No traceability**: nobody can show which changes went through a specification, a design check and an acceptance test.

Today each developer copes with prompts, personal checklists and manual review. Every agent works differently, nothing is checked deterministically, and the process lives in the chat instead of the repository.

## Target Users

- **Levix Digital developers (primary, now)**: build Levix products (Gedo first) with agents in several environments; want one cycle that guides them from idea to release and keeps decisions in the repository.
- **Developers and small teams using coding agents (after the public launch)**: work with Claude Code, Codex, Gemini, Copilot or similar; want structure, gates and review without adopting a platform or paying for a service.
- **Engineering teams and companies (through the Levix platform, a separate product)**: need coordination across machines, managed infrastructure and compliance evidence on top of the same engine.

## Value Proposition

- One entry point (`/dev-cycle` today, `dcp` from v3.0.0) that diagnoses the repository and says where to start; the user decides, the protocol conducts.
- Deterministic gates (`0`, `3`, `rc`, `8`) and a merge gate instead of trusting the agent's word; from v3.0.0 the engine starts each phase itself, so a phase cannot be skipped.
- Roles (product, planner, architect, reviewer, QA, DevOps, writers), each from v3.0.0 in its own context, with its own permissions and with the agent and model chosen for it, so the one who reviews is not the one who wrote.
- Decisions, plans, backlog and roadmap as files in the repository, shared by every agent and developer.
- No lock-in to an agent or a model: plain Markdown, one portable binary, the coding agents the developer already has, any environment, GitHub and GitLab today.
- With Gedo, parallel agents per milestone over entity-level streams; without Gedo, the same cycle runs in sequence.

## Positioning

The approved customer-facing words; the pitch in `docs/product/PITCH.md` copies the one-line description from here verbatim.

- **Tagline**: The engineering process for your coding agents.
- **One-line description**: The agent writes the code. Dev Cycle Protocol makes sure it was done the right way: specification, plan, tests, review and release, with no steps skipped.
- **Is it an agent?** No. The coding agent reasons and writes the code; Dev Cycle Protocol says which phase the agent is in, what it may do next, and checks each step.
- **Is it a harness?** Not of the model: it does not run the model, manage its context or call its tools. It conducts the process: from v3.0.0 its engine starts the coding agent for each role, limits what the role may do and checks the result (ADR-0008).
- **For a technical audience**: a development cycle protocol for coding agents, with an engine that enforces the phases and the gates; model- and tool-agnostic.

## Business Model

Proprietary. The Dev Cycle Protocol and Gedo are two engines in separate repositories. The paid Levix platform (Team Cloud as a service, Enterprise self-hosted) joins them in a visual and cloud layer: managed relay, real-time pull request service, synced memory, the team dashboard Mission Control and compliance reports. Since 2026-11-14 (owner's decision) the protocol is proprietary: the source code is visible to customers only for audit, the repository is private, and binaries are published in the releases repository (ADJ-005). Versions published under MIT OR Apache-2.0 between 2026-11-07 and engine 0.46.1 stay under those licenses. The name's trademark is registered before the public launch (ADJ-004). Price per developer is a hypothesis of the platform, not of this product.

## Success Metrics

- **Lead time**: median time from an approved plan (gate 3) to the merge goes down release after release; baseline measured in v2.9.0.
- **Rework after gates**: fewer than 1 in 5 pull requests needs a second review round for a blocking finding.
- **Escaped bugs**: no critical or high bug found after a release that its release candidate should have caught.
- **Zero-friction install**: every skill runs without Python or `pip install` (since v2.10.0).
- **Adoption (after the public launch)**: number of repositories with a `.dev-cycle.json`.

## Scope

- **Cycle and gates**: the entry command, phases 0 to 10, gates and the merge gate, configuration in `.dev-cycle.json`; from v3.0.0 the engine as conductor, with phase contracts, the cycle trail and one CI job that verifies it.
- **Product and planning**: product inception, backlog with epics, roadmap of releases, versioned plans with milestones and their dependencies.
- **Pull requests and review**: the pull request as the channel, the protocol's code review, on GitHub, GitLab, Bitbucket, Azure DevOps and Gitea.
- **Multi-agent orchestration**: each role in its own context; with Gedo, parallel milestones over streams and deep integration of Gedo's checks into the gates.
- **Agents and environments**: adapters for the main coding agents (Claude Code, Codex, Antigravity CLI, Copilot CLI, OpenCode) and any agent that speaks the Agent Client Protocol, on a local machine and in cloud sessions; a terminal client and a minimal chat screen.
- **Zero-friction adoption**: execution without manual installs, onboarding in three levels and the public launch.
- **Quality, release and documentation**: test strategy and manual test runs, branching models, CI policy, releases, living technical docs and user READMEs.
- **Memory**: plans shared by the team and each developer's session memory.

## Non-Goals

- No paid service required to use the protocol.
- No code specific to one environment: what depends on a provider covers the main three or four.
- No AI attribution in commits, pull requests, reviews or docs (principle 9).
- No dependency on a single coding agent, and no model runtime of its own: the protocol never calls model APIs or stores model credentials.
- No parallel agents without Gedo: without it, everything runs in sequence.
- No merge with the Gedo repository: integration only through Gedo's command line with `--json`.
- Not the platform: the dashboard, editor extension, managed relay, cloud memory and enterprise features are separate products. The protocol's own screen is only a minimal chat (ADR-0010).

## Risks and Assumptions

- **Gedo timing**: parallel work needs Gedo v0.5.0 (local streams and agent checkouts; v0.4.0 brings only the command line) and Gedo support for the project's language (Python in v0.6.0). Mitigation: role isolation in sequence ships first (v3.0.0); nothing in a release depends on an unreleased Gedo version.
- **Agent and platform drift**: agent command lines, the Agent Client Protocol and platform APIs change (GraphQL refused in some environments, for example). Mitigation: REST first, provider adapters behind one model, tests with recorded responses, and from v3.0.0 a scheduled conformance test against the newest version of each verified agent (ADR-0009).
- **Process weight**: the full cycle may feel heavy for small changes. Mitigation: the short path, and onboarding in three levels (FEAT-006).
- **Engine distribution**: the launcher needs the engine binary of its version, downloaded from the release (network access to GitHub on first use) or built with `cargo`. Mitigation: a checksum-verified cache per version, `DEV_CYCLE_BIN` for a binary built elsewhere, and the Python environment friction it replaced is gone (IMP-005, ADR-0007).
- **Assumption**: teams will keep decisions and plans in the repository as files rather than in an external tool.

## Open Questions

- When the public launch happens; planned with Gedo v0.5.0 or v0.6.0. The license is proprietary with source visible for customer audit (ADJ-005); its final text is pending.
- Whether the native verifiers move to Rust next to Gedo; decided in an ADR once Gedo v0.4.0 exists.
