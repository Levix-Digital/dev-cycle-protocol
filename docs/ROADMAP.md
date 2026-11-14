# Roadmap

The releases of the product in delivery order, from the MVP onwards. Each release has a goal and the backlog items (`docs/BACKLOG.md`) it delivers; the roadmap is the only place that says which item goes into which release. Discovery starts from the open items of the current release (the first one not released), after the bugs that come first.

Each release: `## vX.Y.Z: <name>` with the fields `Status` (planned, in_progress, released), an optional `Target`, `Released` (the date, once released), `Goal` and `Items` (backlog ids separated by commas, `-` when empty). Edit this file with `plan-memory/roadmap.py` or by hand, and check it with `roadmap.py validate`.

## v2.10.0: Native engine
- **Status**: planned
- **Goal**: Every script runs as a command of one Rust binary with exactly the same behavior, and the protocol needs no Python
- **Items**: IMP-005, BUG-005, BUG-006, BUG-003, IMP-013

## v2.10.1: Release approval enforced
- **Status**: planned
- **Goal**: No release is published without the user's approval: every publishing environment has a required reviewer, checked at the release gate, and the agent never runs a publishing workflow
- **Items**: IMP-014

## v2.11.0: Foundations for the conducted cycle
- **Status**: planned
- **Goal**: The engine binary is dcp and its printed texts name its commands, Spec Kit comes inside the dcp binary, offline, with no uv, Python, bash or PowerShell in the project, the developer always sees which phase, step and gate the protocol is on through standard status lines, Aurora Dev Suite reads each project's cycle state through dcp diagnose --json and its settings through dcp config, providers such as Aurora Dev Suite join the cycle through a generic extension point, and release candidates are published automatically after the merge gate
- **Items**: ADJ-001, ADJ-003, ADJ-002, IMP-011, IMP-012, FEAT-028, FEAT-019, FEAT-017, FEAT-018, IMP-015

## v3.0.0: Conducted cycle
- **Status**: planned
- **Goal**: The dcp engine conducts the coding agents through the cycle: it decides the phase, starts the agent and model chosen for each role in its own context and with only that phase's permissions, checks every result itself and makes the commits and pull requests, so a phase cannot be skipped; five coding agents are verified and any agent that speaks the Agent Client Protocol can be added; the onboarding proves the setup before the first cycle; the cycle runs from the terminal on a local machine or in a cloud session; one CI job verifies the trail of every pull request; and the skills mode is removed
- **Items**: FEAT-034, FEAT-035, FEAT-036, FEAT-037, FEAT-038, IMP-017

## v3.1.0: Chat screen
- **Status**: planned
- **Goal**: The developer runs the cycle from a minimal chat screen with attachments, questions and gate approvals, and Aurora Dev Suite shows the same chat component in its project view
- **Items**: FEAT-039

## v3.2.0: Guardrails for agent-written code
- **Status**: planned
- **Goal**: Pull requests are blocked on invented or vulnerable dependencies, weakened tests, changes outside the plan and insecure code, as checks of the phase contracts, plans declare the dependencies between milestones, onboarding has three levels, and this repository runs on the protocol itself
- **Items**: FEAT-011, IMP-006, IMP-007, FEAT-012, IMP-002, IMP-004, FEAT-006

## v3.3.0: Cycle feedback
- **Status**: planned
- **Goal**: Production problems come back as triaged bugs, the user's gates come with a review brief, the cycle measures itself automatically at the end of every phase and offers its metrics and a readiness score to Aurora Dev Suite, performance budgets are enforced, each risky change carries a threat model, the release candidate is attacked with its findings coming back as triaged bugs, CI failures and new issues come back every night as triaged bugs, and each role keeps the lessons of past cycles in a LanceDB store that can live locally, in remote storage or in the repository, and a change that touches another project's contracts holds the cycle until every involved project has approved it, and every network destination of dcp is declared and opened through the Levix network gateway
- **Items**: FEAT-013, FEAT-014, FEAT-015, FEAT-022, IMP-010, FEAT-031, FEAT-016, FEAT-021, FEAT-020, FEAT-032, FEAT-033, IMP-016

## v3.4.0: Platforms and infrastructure
- **Status**: planned
- **Goal**: Pull request review works on Bitbucket, Azure DevOps and Gitea, and the project's infrastructure as code is checked in CI and applied through the release gates
- **Items**: FEAT-001, FEAT-010

## v3.5.0: Gedo checks
- **Status**: planned
- **Goal**: With Gedo v0.4.0 enabled, the diagnosis, drift detection, design checks, review and gate 8 use Gedo's checks through its versioned --json contract, and skip them safely when a capability is missing
- **Items**: FEAT-004

## v3.6.0: Parallel work on Gedo
- **Status**: planned
- **Goal**: With Gedo v0.5.0 enabled (agent streams, local intents and gedo scope: FEAT-129, FEAT-130 and FEAT-133 there), independent milestones and specialists run in parallel on Gedo agent streams after gate 3 confirms their scopes do not overlap
- **Items**: FEAT-003, FEAT-008

## v3.7.0: Independent verification
- **Status**: planned
- **Goal**: Every phase declares whether it runs with full or isolated context, the specification and plan are reviewed before gate 3 by reviewers that never saw the conversation, against testable pass rules, and a reviewer that did not run never counts as zero findings
- **Items**: FEAT-023, FEAT-024, FEAT-025, IMP-009

## v3.8.0: Living context
- **Status**: planned
- **Goal**: Documents that reference code are checked against it through Gedo entity identity, and the cycle's context comes from a curated, reviewed map of the project indexed by AnyContext
- **Items**: FEAT-026, FEAT-027
