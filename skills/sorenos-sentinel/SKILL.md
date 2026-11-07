---
name: sorenos-sentinel
description: Autonomous health sentinel for SorenOS ecosystem — audits lease state, monitors active PRs, checks synthetic lab tests, and surfaces blocking conditions.
category: dev
---

# SorenOS Sentinel

Autonomous sentinel and monitor for the SorenOS ecosystem (`Soren.Moussavian`, `SorenOS-PR190-Lab`, `SorenOS-Runtime`, and `SorenOS-Aeon`).

## Purpose

1. Regularly audit the SorenOS lease and control state on branch `sorenos-meta`.
2. Monitor active PRs (e.g. PR #190, PR #191) across SorenOS repositories.
3. Validate CI run statuses across the ecosystem and detect stranded leases or failed fenced mutations.
4. Update `memory/topics/sorenos-status.md` and log telemetry to `memory/logs/`.

## Steps

### 1. Audit SorenOS Control State & Lease
Run `gh` to inspect branch `sorenos-meta` in `SorenMoussavian/Soren.Moussavian`:
- Check the latest commit message and author.
- Verify whether the lease is `FREE` or `HELD`.
- Check if any mutation was aborted or left orphaned (e.g. `FREE->FREE forbidden`).

### 2. Check Active Pull Requests
Inspect open PRs in `SorenMoussavian/Soren.Moussavian`:
- Verify PR states, labels, and pre-merge check statuses (e.g. CodeRabbit review results, Docstring Coverage).
- Check Vercel preview deployment status and commit-verification policy.

### 3. Check Synthetic Lab Status
Inspect `SorenMoussavian/SorenOS-PR190-Lab`:
- Check status of recent runs of `PR190 Periodic Synthetic Replay & CI`.
- Verify runtime manifest checksums and invariant tests.

### 4. Record Evidence and Update Topics
Write the consolidated audit to `memory/topics/sorenos-status.md`.
Append audit summary to `memory/logs/${today}.md` under `### sorenos-sentinel`.
