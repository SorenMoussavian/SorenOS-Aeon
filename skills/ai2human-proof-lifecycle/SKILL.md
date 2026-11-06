---
name: ai2human-proof-lifecycle
description: Inspect and report an AI2Human task through claim, human proof submission, verification, funding, settlement, refund, expiry, and dispute states. Use when a task already exists or when determining whether work is truly complete and paid.
---

# AI2Human Proof Lifecycle

Use the canonical loop:

`task -> human claim -> human evidence -> proof review -> verified -> settlement submitted -> settled`

## State Rules

- Creation is not execution.
- Claim is not proof.
- System logs are not human evidence.
- Proof submission is not verification.
- Verification is not settlement.
- A displayed reward is not funding.
- Payment is complete only when a recorded payment/transaction receipt exists.

## Report

Return task ID/URL, workflow state, task state, human evidence count, verification state, funding state, settlement state, and transaction hash when present. Clearly label `test_no_payout`, `awaiting_funding`, expired, disputed, refunded, or needs-review states.
