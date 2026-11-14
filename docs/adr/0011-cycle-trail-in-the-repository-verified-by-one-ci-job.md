---
id: 11
title: Cycle trail in the repository verified by one CI job
status: accepted
date: '2026-11-14'
version: v3.0.0
subsystems: [cycle, quality-release]
supersedes: []
superseded_by: []
amends: []
amended_by: []
---

# ADR-0011: Cycle trail in the repository verified by one CI job

## Context

With ADR-0008 the engine guarantees the cycle only for work that goes through it; nothing stops a push of code that never did. Customers also need proof, at merge time, that each change went through the protocol. FEAT-030 planned Git hooks, agent hooks and a required CI check, and FEAT-029 planned instruction blocks and hooks written into the user's repository. The owner wants a minimal footprint in the user's repository and a CI check that is permanent but effortless on GitHub, GitLab, Azure DevOps, Bitbucket and Gitea, with complex pipelines or none.

## Decision

- In the user's repository the protocol keeps only: `.dev-cycle.json`, the project artifacts (backlog, roadmap, plans, ADRs), the cycle trail and one CI job. Protocol skills, Spec Kit files, instruction blocks and agent hooks are not written there (ADR-0008).
- The cycle trail is the record of every completed phase, check and approved gate, with the agent and model of each phase. It lives in files in one folder of the protocol, one file per cycle so parallel cycles do not conflict. Each entry is chained to the previous one and to the state of the code by a digest.
- One isolated CI job verifies the trail of each pull request. It downloads the `dcp` binary (from the releases repository, or from the customer's internal repository when there is no internet access) and runs the verification. This job is the only merge-time check; there is no status posted from the developer's machine.
- `dcp ci install` detects the platform, generates the job and opens a pull request with it: a new workflow file on GitHub and Gitea; a new file plus the pipeline registration and branch policy through the API on Azure DevOps; one include line in the single pipeline file on GitLab and Bitbucket. Existing workflows are never edited beyond that.
- `dcp` turns on the requirement at merge through the platform API when the developer has permission; otherwise it shows the administrator the exact step. The diagnosis reports the missing job or requirement as a permanent pending item, without blocking local use.
- When the repository is governed by an Admin Hub, the job also verifies that each gate in the trail carries a valid Hub attestation for the commit it approves, with the Hub key fingerprint pinned in `.dev-cycle.json`.

## Alternatives Considered

- **A commit status posted by the engine from the developer's machine, required by branch protection**: no workflow file at all, but anyone with the developer's credentials can forge it; two mechanisms to maintain if kept next to the job.
- **The trail in Git references outside the work tree**: smaller footprint, but such references are not pushed by default, platforms do not show them in a pull request, and a squash merge detaches them from the commits.
- **No CI check**: zero footprint in the pipeline, but no guarantee and no audit proof at merge.
- **Git and agent hooks (FEAT-030 as planned)**: local enforcement, but hooks are skipped easily and add files to the repository.

## Consequences

- The verification runs outside the developer's machine, so it cannot be forged from it, and it is the audit evidence for customers.
- Without a key outside the repository, the digest chain detects accidental edits and edits by an agent, not someone determined to rebuild the whole chain; the strong proof is the Hub attestation for customers that use a Hub.
- A project with no CI available has no merge-time guarantee; the diagnosis keeps saying so.
- GitLab and Bitbucket need one line in an existing file; that is the smallest possible change there.
- Verify: on each supported platform, a pull request whose trail is missing, broken or does not match the code fails the job, and one produced by a full cycle passes.
