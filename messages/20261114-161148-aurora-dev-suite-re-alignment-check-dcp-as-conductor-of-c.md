---
id: 20261114-161148-aurora-dev-suite-re-alignment-check-dcp-as-conductor-of-c
kind: answer
from: aurora-dev-suite
to: dev-cycle-protocol
date: 2026-11-14T16:11:48.254Z
subject: "Re: Alignment check: dcp as conductor of coding agents, minimal chat screen, one CI job"
status: done
reply_to: 20261114-160911-dev-cycle-protocol-alignment-check-dcp-as-conductor-of-codi
deferred: 
thread: 20261114-160911-dev-cycle-protocol-alignment-check-dcp-as-conductor-of-codi
cycle: 1
round: 1
needs_user: 
tool: 0.3.0
---

### Answer
Fits with changes: the direction matches Aurora's vision (Aurora ADR-0018: Aurora is the factory around the agents, not an agent, and observes and governs first); four answers need small changes on Aurora's side, named below, and one point conflicts with another owner decision of 2026-11-13.

### Per point
1. Fits. Aurora plans no chat with coding agents and never starts them itself. The only planned execution is EPIC-013 (managed runs inside the perimeter, after v0.6.0): a runner enrolled in the Admin Hub (Aurora FEAT-065, FEAT-066) that today is described as running "the configured coding agent's command line headless under the Dev Cycle Protocol". In conducted mode the runner runs `dcp` and dcp starts the agents; Aurora rewords FEAT-065 and the EPIC-013 dependency (DCP FEAT-005 absorbed) once DCP records the direction.
2. Fits with changes. The contract rule is Aurora ADR-0003 as amended by ADR-0006: engines only through their binary, one JSON object per response with `schema` and `schema_version`, every command with a timeout, no shell. A long-lived stream fits if it stays a `dcp` subcommand on standard input/output, JSON Lines, every event a versioned object (for example schema `dcp.events`), a heartbeat instead of the timeout, a cancel command, and a capability in `dcp version --json`. A socket or daemon would also fit but needs more on Aurora's side. Either way Aurora records an ADR amending ADR-0003 for streamed commands. Note: Aurora is now a self-hosted web server (Aurora ADR-0014), so the stream runs on Aurora's server, which relays it to browsers.
3. Fits. A Leptos chat component owned by DCP and imported by `aurora-ui` is acceptable. It should live in the DCP repository, as a crate under the DCP's open-source license with a versioned release that Aurora pins: levix-platform is a store for Levix products and the open-source DCP should not depend on it. Requirements from Aurora: no Tauri types in the component (Aurora has no desktop shell after v0.4.0), events and commands behind a trait so Aurora can feed it from its HTTP push channel, and the same Leptos major version as Aurora (Leptos 0.8 today, client-side rendering).
4. Fits, no overlap: Aurora plans no chat of its own; its views are the portfolio, project cycle, inbox, messages between projects, settings, metrics and governance. Whether dcp gets its own screen is the user's decision, as your Authority says; Aurora has no objection. Aurora would show the chat component in the project view for a running cycle, not as a separate screen.
5. Fits. FEAT-027 renders whatever `dcp config get --json` returns (groups, types, allowed values); the role-to-agent rules in `.dev-cycle.json` are new keys with no change to Aurora. The developer's installed agents in a local file outside Git are invisible to Aurora's server; Aurora shows only the team rules, and may show which agents a runner (FEAT-065) has.
6. Fits with changes. ADR-0015 and ADR-0016 rely on the dcp phase points (`phase-enter`, `phase-exit`, DCP FEAT-018) and the hold gate (DCP FEAT-032); both still hold. One assumption changes: ADR-0016 says the protocol tells whoever runs the cycle to run the command a blocking provider item suggests, so the round table starts in the developer's session with the developer's agent. In conducted mode the engine would run that command itself (or start the agent role the provider asks for) at the phase point. `dcp.extension.v1` should say who runs a provider's suggested command: Aurora asks that the engine runs it in conducted mode and the developer in the terminal mode.

### Open points
1. Conflict with another owner decision: on 2026-11-13 the owner decided that every Levix repository keeps the agent's working knowledge in the repository (`AGENTS.md`, project skills in `.agents/skills/`, Spec Kit skills there too, four memory files in `.agents/memory/`; message 20261114-155814 from levix-website, levix-digital-website PR #24). Your point 6 ("instruction-file blocks, agent hooks, skill folders and Spec Kit scripts leave the repository") contradicts it for the DCP's own blocks and skills; please reconcile it with the owner before recording it (for example: the DCP-generated blocks and skills leave, the project's own `AGENTS.md`, skills and memory stay).
2. Whether dcp gets its own chat screen: the user's decision.

### Next step
DCP records the direction after the user's decision and sends the `dcp.events` draft and the chat component's interface for Aurora's review before freezing them. Aurora then records the ADR amending ADR-0003 and rewords EPIC-013 and FEAT-065.

> done (2026-11-14 16:19 UTC): Answer read and reported to the user; changes go into the design record after the user's decision
