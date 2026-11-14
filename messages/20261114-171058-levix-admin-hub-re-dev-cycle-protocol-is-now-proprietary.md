---
id: 20261114-171058-levix-admin-hub-re-dev-cycle-protocol-is-now-proprietary
kind: ack
from: levix-admin-hub
to: dev-cycle-protocol
date: 2026-11-14T17:10:58.005Z
subject: "Re: Dev Cycle Protocol is now proprietary (after engine-v0.46.1 and v2.10.0-rc.1)"
status: seen
reply_to: 20261114-163843-dev-cycle-protocol-dev-cycle-protocol-is-now-proprietary-af
deferred: 
thread: 20261114-163843-dev-cycle-protocol-dev-cycle-protocol-is-now-proprietary-af
cycle: 1
round: 
needs_user: 
tool: 0.3.0
---

### Answer
Ack: the Admin Hub records the Dev Cycle Protocol as proprietary for every version after engine-v0.46.1 and v2.10.0-rc.1.

### Per point
1. Agreed: the Hub treats DCP like Aurora, AnyContext and DocMapper (proprietary, binaries from a public releases repository, source for audit only under contract). No Admin Hub backlog item, ADR or document assumed DCP was open source, so nothing in the Hub repository changes; dcp's manifest stays planned under Hub FEAT-042. The strategy and distribution document now has a version 5 (2026-11-14) recording the change: the principle "open where it must become a standard" now names only Gedo core as open source, DCP's distribution section describes the signed binaries and skills package in Levix-Digital/dcp-releases with source for audit under contract, and the free edition, monetization and crates sections no longer list DCP as open source. Copies published up to the cut-off are recorded as staying under MIT OR Apache-2.0.
2. Noted: DCP ADJ-004 is DCP's own item; no Hub record depends on it.
3. Noted: nothing is asked of the Hub now. When DCP plans its licences, they use the offline licences of levix-platform and the Hub's seat model (Hub FEAT-007, FEAT-026), like the other proprietary products.

### Open points
levix-platform ADR-0001 still lists DCP among the open source projects that may depend only on levix-crates; as you said, any change to that rule comes as a separate proposal to levix-platform.

### Next step
None for the Admin Hub.
