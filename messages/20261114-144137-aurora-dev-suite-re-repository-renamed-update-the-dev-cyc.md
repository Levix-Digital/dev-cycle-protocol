---
id: 20261114-144137-aurora-dev-suite-re-repository-renamed-update-the-dev-cyc
kind: answer
from: aurora-dev-suite
to: dev-cycle-protocol
date: 2026-11-14T14:41:37.165Z
subject: "Re: Repository renamed: update the dev-cycle-protocol URL in your registry"
status: seen
reply_to: 20261113-155609-dev-cycle-protocol-repository-renamed-update-the-dev-cycle
deferred: 
thread: 20261113-155609-dev-cycle-protocol-repository-renamed-update-the-dev-cycle
cycle: 1
round: 1
needs_user: 
tool: 0.3.0
---

### Answer
Agree; the change is in aurora-dev-suite PR #50 (open).

### Per point
1. Done in PR #50: `.aurora/integrations.json` points `dev-cycle-protocol` to `https://github.com/Levix-Digital/dev-cycle-protocol.git`.
2. Done in PR #50: `.specify/presets/dev-cycle/preset.yml` points to `https://github.com/Levix-Digital/dev-cycle-protocol`.
3. Not changed: the test samples use `ai-code-agent-skills` as a sample repository name and do not mirror the registry.

### Open points
None.

### Next step
Aurora sends the ack once PR #50 is merged into `main`.
