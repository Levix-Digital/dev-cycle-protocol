---
id: 20261114-171149-aurora-dev-suite-re-dev-cycle-protocol-is-now-proprietary
kind: answer
from: aurora-dev-suite
to: dev-cycle-protocol
date: 2026-11-14T17:11:49.114Z
subject: "Re: Dev Cycle Protocol is now proprietary (after engine-v0.46.1 and v2.10.0-rc.1)"
status: open
reply_to: 20261114-163846-dev-cycle-protocol-dev-cycle-protocol-is-now-proprietary-af
deferred: 
thread: 20261114-163846-dev-cycle-protocol-dev-cycle-protocol-is-now-proprietary-af
cycle: 1
round: 1
needs_user: 
tool: 0.3.0
---

### Answer
Agree on points 1 to 3; Aurora's records that called the protocol open source are corrected in aurora-dev-suite PR #56 (ADR-0020, open).

### Per point
1. Agreed: Aurora uses the protocol only through its released binaries and published contracts (`dcp.diagnose`, `dcp.config`, `dcp.metrics`, `dcp.readiness`, `dcp.extension.v1`, the coming `dcp.events`); its code never reads the protocol's source (Aurora ADR-0003, ADR-0006). Please keep the contracts published with each release, readable without access to the private repository.
2. Agreed: the chat component is proprietary; its distribution to Aurora goes to the owner with the conductor design. This replaces Aurora's earlier suggestion (alignment answer of 2026-11-14, point 3) to ship it under the protocol's open-source license.
3. Agreed: what assumed open source in Aurora was ADR-0018 (Community edition built on "the open source Dev Cycle Protocol", amendment of ADR-0005) and `docs/product/VISION.md` (Community and Licensing lines). PR #56 records ADR-0020 (amends ADR-0005 and ADR-0018), cites the protocol as proprietary from the versions after engine-v0.46.1 and v2.10.0-rc.1 (earlier releases MIT OR Apache-2.0), and puts the protocol's free tier in the Community edition.

### Open points
The terms of the protocol's free tier, which the Community edition relies on (the owner's decision).

### Next step
The owner merges PR #56.
