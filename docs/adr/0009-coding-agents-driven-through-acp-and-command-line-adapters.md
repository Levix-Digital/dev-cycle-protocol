---
id: 9
title: Coding agents driven through ACP and command-line adapters
status: accepted
date: '2026-11-14'
version: v3.0.0
subsystems: [cycle]
supersedes: []
superseded_by: []
amends: []
amended_by: []
---

# ADR-0009: Coding agents driven through ACP and command-line adapters

## Context

ADR-0008 makes the engine start a coding agent for each role. Coding agents differ in how they are started, how they return output and how they limit permissions, and they change often. Checked on 2026-11-14: Antigravity CLI (`agy acp`), OpenCode (`opencode acp`) and Copilot CLI (`copilot --acp`, public preview) speak the Agent Client Protocol (ACP) natively; Claude Code and Codex have a non-interactive command line with JSON output and reach ACP only through separate adapters. In ACP every permission request of the agent goes to the client, which allows or rejects it. ACP has a version 2 in draft.

## Decision

- The engine drives agents through two adapter kinds behind one interface of four operations: start a role with its package, receive events, answer a question, stop.
- The ACP adapter is the preferred path: the engine is the ACP client and decides every permission request according to the phase.
- The command-line adapter covers agents without native ACP: it uses the agent's non-interactive mode, its structured output and its own permission options.
- A built-in catalog, as data, describes each agent: name, how to start it, how to detect it, how to check its sign-in and how to pass the model. Entries are `verified` (tested by this project at every release) or `listed` (expected to work through ACP, not tested here). The user adds custom entries in the same format.
- Verified from the first conducted release: Claude Code and Codex (command line), Antigravity CLI, Copilot CLI and OpenCode (ACP).
- A role is served by an agent, a model and an effort level, so different phases can use different models of one agent or different agents. If the configured model is not available the engine stops and says so; it never falls back silently.
- A scheduled job in this repository installs the newest version of each verified agent and runs a short conformance test (start a role, structured output, a denied permission is respected, a question round trip). A failure becomes a bug. The catalog keeps the tested version range, and `dcp` warns when the installed agent is outside it.
- In an environment governed by an Admin Hub, the engine starts the agents through the Levix network gateway's pre-start check and passes its proxy settings to them, so the agents' model endpoints are checked against the policy (IMP-016).

## Alternatives Considered

- **ACP only**: one adapter, but Claude Code and Codex would need a separate adapter installed by the user.
- **Command line only**: works everywhere today, but permission control depends on what each agent offers, and some agents do not pass permission questions to the caller in that mode.
- **Agent SDKs or model APIs**: finer control, but one integration per provider and API keys instead of the developer's sign-in.

## Consequences

- Any ACP agent can be used with no new code; verified agents cost maintenance, paid by the conformance job and by keeping a credential for each one in this repository's CI.
- The permission guarantee is stronger on ACP agents than on command-line agents; the catalog states which kind each agent is.
- The protocol depends on a moving standard (ACP) and on agent command lines that change; the tested version range makes drift visible.
- This replaces FEAT-005 (role runners) and extends ADR-0003's approach of provider adapters behind one model to coding agents.
- Verify: the conformance test passes for the five verified agents on the three platforms, and a role assigned to a model that is not available stops with a clear message.
