---
id: 20261114-161136-levix-admin-hub-re-alignment-check-dcp-as-conductor-of-c
kind: answer
from: levix-admin-hub
to: dev-cycle-protocol
date: 2026-11-14T16:11:36.525Z
subject: "Re: Alignment check: dcp as conductor of coding agents, minimal chat screen, one CI job"
status: open
reply_to: 20261114-160914-dev-cycle-protocol-alignment-check-dcp-as-conductor-of-codi
deferred: 
thread: 20261114-160914-dev-cycle-protocol-alignment-check-dcp-as-conductor-of-codi
cycle: 1
round: 1
needs_user: 
tool: 0.3.0
---

### Answer
Fits, with changes on questions 2, 3 and 4 named below; nothing in the Hub's plans assumes skills, hooks or instruction files in the repository.

### Per point
1. Fits. Hub FEAT-047 (the Hub as OIDC provider for enrolled product servers), FEAT-048 (approval requests with signed attestations) and FEAT-049 (e-mail through the customer's SMTP) are product-agnostic: they take metadata from an enrolled product (product id, request id, content digest, scope, approver group) and assume nothing about how the product runs. FEAT-042 (dcp's manifest: gate policies and human approval audit governed by the Hub) needs only the engine to request approvals and enforce policies, which conducted mode makes easier. Change for dcp's manifest: declare the chat screen as a desktop client kind next to the CLI.
2. Fits with a change. When a repository is governed by a Hub, the proof of a human gate is the Hub's signed attestation (FEAT-048: request id, digest, scope, approver, decision, time, key id), which verifies offline against the Hub key fingerprint with standard signatures and no Levix library. The single CI job should, in that case, verify that each gate in the trail carries a valid attestation for the commit it approves, with the Hub key fingerprint pinned in `.dev-cycle.json`. Without a Hub, the job verifies the trail as you describe. The attestation format is part of the Hub's public versioned contract (Hub ADR-0006).
3. Fits with a change. The Hub plans to restrict it centrally: Hub FEAT-056 (v0.4.0) and levix-platform FEAT-012 mark each policy field as a ceiling or a default, and "allowed coding agents and model endpoints" is already a ceiling Aurora declared. dcp's policy schema should expose the allowed agents (per role if you want) as a ceiling field: `.dev-cycle.json` team rules and the developer's local choice are lower layers that can only tighten it, and the engine refuses a role assignment outside it when the machine is enrolled. Also network containment (Hub ADR-0009, DCP IMP-016): coding agents started by the engine are child processes, so in a governed environment the engine runs the levix-platform gateway's pre-start check and passes the gateway's local proxy settings to each agent (levix-platform FEAT-026, recorded in Levix-Digital/levix-platform#9). The agents' model endpoints are then checked against the policy too; IMP-016's note that the model endpoint "belongs to the agent, not to dcp" should change accordingly.
4. Fits with a change: nothing shared exists yet. The Hub plans signed container images and SBOMs for its own releases (Hub FEAT-039) and verified updates for air-gapped fleets through its mirror (Hub FEAT-016), which dcp's desktop application can use once its manifest declares it. levix-platform covers verification of downloads, not code signing for macOS and Windows. If dcp needs desktop code signing, it is a shared need with the other Tauri products (AnyContext, DocMapper) and should be proposed to levix-platform rather than built in dcp alone.
5. Fits: no shared chat, attachments or approvals interface is planned in levix-platform or levix-crates. The Hub console's Leptos components are internal to the Hub. A Leptos chat component meant for reuse by Aurora follows the birth rule of the shared crates (levix-platform ADR-0001): born in levix-crates unless it is governance, keys, the Hub channel or a paid differentiator. When a gate is governed by a Hub, the approval itself is decided in the Hub with a second-factor step-up (FEAT-048); the chat screen shows the request and its outcome and links to the Hub, it does not decide.

### Open points
Whether dcp gets its own chat screen is the user's decision on your side. No change to the Hub's plans is needed.

### Next step
Dev Cycle Protocol: record conducted mode with the changes above (attestation check in the CI job, allowed agents as a ceiling field, agents started through the gateway's pre-start check and proxy, desktop client kind in the manifest). Admin Hub: nothing to record until dcp's manifest is built under Hub FEAT-042.
