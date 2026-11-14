---
id: 10
title: One engine interface for the terminal, the chat screen and Aurora
status: accepted
date: '2026-11-14'
version: v3.0.0
subsystems: [cycle]
supersedes: []
superseded_by: []
amends: []
amended_by: []
---

# ADR-0010: One engine interface for the terminal, the chat screen and Aurora

## Context

ADR-0008 makes the engine the place where the cycle runs, so every way of using it must go through the engine. The terminal is poor for attaching files and writing long instructions, and a rich terminal interface is costly to build. Aurora Dev Suite reads the engine only through the `dcp` binary with one JSON object per response (Aurora ADR-0003 and ADR-0006), is a self-hosted web server with a Leptos interface and no desktop shell, and plans no chat of its own. The owner had decided on 2026-10-28 that the protocol has no graphical interface of its own.

## Decision

- The engine exposes one interface of events and commands, and every client uses it with no logic of its own: the terminal, the protocol's chat screen and Aurora Dev Suite. Clients never start coding agents.
- The interface is a `dcp` subcommand on standard input and output, JSON Lines, every event a versioned object, with a heartbeat, a cancel command and a capability in `dcp version --json`. Aurora reviews the draft before it is frozen.
- The terminal client is part of the `dcp` binary: it starts and resumes a cycle and shows status lines (FEAT-028), questions and gates as text. It is the client for remote machines, cloud sessions and scripts.
- The protocol gets a minimal chat screen of its own: chat, attachments, questions and gate approvals, nothing else. Dashboards, metrics and graphical settings stay only in Aurora. This relaxes the decision of 2026-10-28.
- The chat screen is a Tauri 2 application, a separate artifact with its own installers; `dcp` stays one terminal binary.
- The chat view is a Leptos component in a crate of this repository, released with a version that Aurora pins. The component has no Tauri types, receives events and sends commands through a trait, and uses the same Leptos major version as Aurora, with client-side rendering. Tauri is only the shell of the protocol's application.
- When a gate is governed by an Admin Hub, the approval is decided in the Hub; the chat screen shows the request and its outcome and links to the Hub.

## Alternatives Considered

- **A local web page served by the binary**: no second artifact, but a local server needs its own protection, and file dialogs and drag and drop are weaker.
- **A rich terminal interface**: one binary, but costly to build and still poor for attachments.
- **Graphical interface only in Aurora**: no duplication, but the protocol would need another product to be used comfortably.
- **The chat component in the shared Levix libraries**: follows the birth rule of the shared crates, but the component belongs to the protocol's own interface and is released with it.

## Consequences

- A second artifact to build, sign and distribute; desktop code signing on macOS and Windows is a need shared with other Levix desktop products and is proposed to levix-platform when the chat screen is planned.
- No screen on a remote machine or in a cloud session; the terminal client covers them.
- Aurora shows the same chat component in its project view and amends its ADR-0003 for streamed commands.
- The protocol's manifest for the Admin Hub declares the chat screen as a desktop client kind next to the command line.
- Verify: the same cycle can be started in the terminal, continued in the chat screen and observed from a second client with no difference in state.
