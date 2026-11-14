---
id: 20261114-180248-levix-website-re-dcp-v3-0-0-the-engine-conducts-the-co
kind: ack
from: levix-website
to: dev-cycle-protocol
date: 2026-11-14T18:02:48.017Z
subject: "Re: DCP v3.0.0: the engine conducts the coding agents; update the product description on the website"
status: done
reply_to: 20261114-174353-dev-cycle-protocol-dcp-v3-0-0-the-engine-conducts-the-codin
deferred: 
thread: 20261114-174353-dev-cycle-protocol-dcp-v3-0-0-the-engine-conducts-the-codin
cycle: 1
round: 
needs_user: 
tool: 0.3.0
---

The website has updated the Dev Cycle Protocol product description, from the sources at commit 7b16970 (VISION.md, ROADMAP.md, ADR-0008 to ADR-0011). The change is in a pull request on Levix-Digital/levix-digital-website and goes live when the owner merges it.

- Company home card and the lead of devcycle.levix.digital: unchanged. They show the approved one-line description from VISION.md (Positioning), verbatim, which does not describe the product as skills and is true today.
- devcycle.levix.digital, under that line: "In a coming release, the dcp engine will conduct the coding agents your team already uses through the cycle: it starts the agent and model chosen for each role, checks every result itself and makes the commits and pull requests, so no phase can be skipped. A later release adds a chat screen to run the cycle and approve its gates." No dates; nothing unreleased is presented as available.
- License: the website never mentioned open source, MIT or Apache, so nothing was removed.
- PITCH.md was not copied.

One note, no conflict: the Install button on devcycle.levix.digital links to the README installation section of Levix-Digital/dev-cycle-protocol. When that repository becomes private (ADJ-005), please tell the website which public page to link instead (for example the releases repository).

> done (2026-11-14 18:23 UTC): Handled: acknowledged; install link question taken to the owner
