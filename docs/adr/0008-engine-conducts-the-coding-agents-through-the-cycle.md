---
id: 8
title: Engine conducts the coding agents through the cycle
status: accepted
date: '2026-11-14'
version: v3.0.0
subsystems: [cycle, configuration]
supersedes: [1, 2]
superseded_by: []
amends: []
amended_by: []
---

# ADR-0008: Engine conducts the coding agents through the cycle

## Context

Since ADR-0001 the protocol is a set of skills that the coding agent reads and chooses to follow; the engine (ADR-0007) only checks when the agent calls it. Phases exist only in the skill text, and gates `0`, `3`, `rc` and `8` run only when the agent runs them, so nothing stops an agent from writing code before gate 3 or from skipping a review. The items planned to close that gap (project instruction blocks and session hooks in FEAT-029, Git and agent hooks in FEAT-030, a local MCP server in FEAT-009) all keep the agent in control: each one asks the agent to obey. The owner decided on 2026-11-14 to invert the control. The product is not public yet, so no compatibility period is needed.

## Decision

- The `dcp` engine is the conductor of the cycle. A deterministic state machine in the engine decides the phase, builds the context of the role, starts the coding agent for that role, checks the result and only then advances. The orchestrator is code, never a model; the chat is only the user's interface for questions and gates.
- The engine never trusts the coding agent: not its exit code and not its claim of being done. Only the engine's own deterministic checks decide that a phase passed. Example that made the rule explicit: one agent, in non-interactive mode, silently denies a permission nobody can approve and still exits with code 0.
- The engine does not run models. It drives the coding agents the developer already has, with the sign-in the developer already made, and stores no model credentials (ADR-0009).
- One role, one context, one permission set. Each role runs in a clean context and receives a role package: the instructions of that phase only, the input artifacts and the permissions of the phase (a reviewer only reads; a planner writes no code). The agent returns a structured result: artifacts, questions for the user or done.
- Phase contracts are data in the engine: inputs, artifacts, done criteria, checks, permissions and mode. The mode is `conversation` (inception, clarification, plan: the role's agent stays alive and the user talks to it until asking to conclude) or `autonomous` (implementation, tests, review). In both modes the engine opens the phase, limits the permissions, checks the result and closes the phase.
- The engine does what cannot be undone: commits, pull requests and gates. The user's gates stop the cycle until a recorded approval.
- There is one mode only. The skills mode is removed with no fallback. In a cloud session the session's agent only starts `dcp`, which conducts the role agents as subprocesses inside the container.
- What is exclusive to the protocol lives in the binary: its own skills and the Spec Kit skills, scripts and templates (IMP-011, IMP-012). What belongs to the project stays in the repository (`AGENTS.md`, the project's own skills and memory), and the conducted agents read it as usual. The engine writes no instruction block of its own.
- Code boundaries: the role crates and their checkers stay one crate per role with the small shared kernel, as ADR-0002 defined; the conductor (state machine, phase contracts, adapters) lives in crates of its own, and the checkers never call the conductor.
- Parallel agents stay as in ADR-0005: only with Gedo; without it the roles run in sequence.

## Alternatives Considered

- **Keep the skills and add hooks and a required CI check (FEAT-029, FEAT-030)**: smaller change, but the agent still decides whether to follow the protocol; the engine only detects a skipped phase afterwards.
- **Serve the protocol through a local MCP server (FEAT-009)**: the engine hands out one phase at a time, but the agent still chooses to call it.
- **A model as the orchestrator (a main chat with sub-agents)**: natural for the user, but the orchestrator can skip steps like any agent.
- **A full harness that calls the model APIs**: full control, but it rebuilds what the coding agents already do (tools, context management), needs API keys instead of the developer's subscription and ties the product to model providers.
- **Keep the skills mode as a guided fallback for cloud sessions**: covers agents with no command line, but leaves two modes to maintain and a weaker guarantee in one of them.

## Consequences

- Skipping a phase is no longer possible: the agent of the next phase is not started before the previous one passes.
- ADR-0001 and ADR-0002 are superseded. FEAT-029, FEAT-030, FEAT-009, FEAT-002, FEAT-005 and IMP-008 are replaced by the conducted cycle items of v3.0.0; the positioning in the vision changes from "not a harness" to "does not run the model, conducts the process".
- Every project that uses the protocol today switches at v3.0.0; the coding agents must be installed and signed in wherever the cycle runs, cloud containers included.
- More tokens per cycle, because each role starts from a clean context; the handoff artifacts between phases must carry what the next role needs. The trail records agent and model per phase so the cost is measured.
- Agents that exist only inside an editor, with no command line, cannot be conducted.
- Verify: on the sample projects, a cycle run through `dcp` cannot reach implementation without an approved gate 3, and a role whose agent reports success without producing the artifacts fails the phase.
