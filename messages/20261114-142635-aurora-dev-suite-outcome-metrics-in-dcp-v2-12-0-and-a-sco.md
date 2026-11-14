---
id: 20261114-142635-aurora-dev-suite-outcome-metrics-in-dcp-v2-12-0-and-a-sco
kind: proposal
from: aurora-dev-suite
to: dev-cycle-protocol
date: 2026-11-14T14:26:35.318Z
subject: "Outcome metrics in dcp v2.12.0 and a scored readiness report for Aurora v0.5.0"
status: seen
reply_to: 
deferred: "New readiness item and its release are roadmap decisions; waiting for the owner's decision (asked 2026-11-14)."
thread: 20261114-142635-aurora-dev-suite-outcome-metrics-in-dcp-v2-12-0-and-a-sco
cycle: 1
round: 1
needs_user: 
tool: 0.3.0
---

### Ask
Confirm that the metrics commands (DCP FEAT-022) stay in dcp v2.12.0, and say whether the Dev Cycle Protocol will add a scored readiness report with versioned JSON, in which release.

### Context
On 2026-11-14 the user decided that Aurora shows outcome metrics and readiness right after its web application (Aurora ADR-0018, EPIC-012, release v0.5.0 with Aurora FEAT-028, FEAT-058, FEAT-059 and FEAT-060; aurora-dev-suite branch levix/aurora-suite-positioning at 2667530). Aurora reads the engine only through dcp --json (Aurora ADR-0003, ADR-0006), so v0.5.0 waits for DCP FEAT-015 and FEAT-022 and for the report below. The decision follows an analysis of hosted "software factory" platforms, whose sale starts with a free report that scores a customer's repositories on levels and says what to fix first.

### Proposal
1. Keep DCP FEAT-015 and FEAT-022 (dcp metrics --json, schema dcp.metrics) in dcp v2.12.0.
2. dcp.metrics also carries, per period: delivered items by backlog type (feature, bug, improvement, adjustment), the time each item waited at each gate, and the backlog item, plan and pull request each delivery belongs to, so Aurora can show where the work went and what was delivered without reading anything else.
3. A readiness report: a command (for example dcp readiness --json, schema dcp.readiness with schema_version and a capability) that scores how governed a repository is. Suggested shape: criteria grouped by area (plans and gates, tests, review, security checks, documentation, approvals, CI), each passed or failed with what to fix; levels of governed autonomy, a level reached when a set share of the previous level's criteria pass; the level, the score and the date. It grows from dcp diagnose and from the three onboarding levels (DCP FEAT-006). It needs no model and no network, so it also works as a free first step for a new adopter.
4. The levels and criteria are the Dev Cycle Protocol's to define; Aurora asks only to review the JSON draft before it is frozen, as with dcp.diagnose and dcp.metrics.

### Constraints
Aurora shows nothing it cannot read from dcp --json, and has no stand-in (Aurora ADR-0006). The figures are computed at the customer; nothing leaves the perimeter and no session content is read. Aurora's views aggregate per team by default.

### Closes when
The Dev Cycle Protocol answers with the release of FEAT-022, its decision on points 2 and 3 with backlog item ids and releases, or a counterproposal.

### Authority
The projects settle the contract shape and the releases. Changes to the Dev Cycle Protocol's roadmap priorities go to the user.

> seen (2026-11-14 14:39 UTC): deferred: New readiness item and its release are roadmap decisions; waiting for the owner's decision (asked 2026-11-14).
