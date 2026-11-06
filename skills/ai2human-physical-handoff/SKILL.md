---
name: ai2human-physical-handoff
description: Create AI2Human physical execution tasks for pickups, deliveries, signatures, counter-code handoffs, and bounded errands with receipt or photo proof. Use when a real person must manipulate or transfer a physical item.
---

# AI2Human Physical Handoff

Use `physical_task` for direct physical execution or `errand` for a multi-step trip. Location is required.

## Task Contract

Specify item, authorized pickup/drop-off point, time window, safe handoff method, and failure path. Never request credentials, financial deposits, restricted goods, impersonation, or unsafe access.

## Evidence

Choose only necessary proof: photo, receipt, signature, timestamp, notes. Do not expose private documents or unrelated personal information. Verification confirms the handoff evidence; settlement remains a separate funded transition.
