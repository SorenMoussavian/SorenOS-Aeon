---
name: ai2human-digital-qa
description: Create bounded AI2Human digital QA and reproduction tasks for browser, mobile, device, region, account-state, or app failures that automation cannot conclusively resolve. Use for repeated page failures, explicit error messages, broken UI flows, and screenshot-backed manual testing; not for social engagement.
---

# AI2Human Digital QA

Use category `digital_task` and the natural-task execution contract (`platform: real_world`, `custom-task-spec/v1`).

## Trigger

Dispatch only after deterministic checks and configured retries remain inconclusive or consistently fail. Record the attempts in the brief.

## Required Task Shape

- Exact target URL/build/environment.
- Reproduction steps and expected result.
- Device/browser/region constraints only when needed.
- Proof: `screenshot`, `timestamp`, `notes`; add `url` when the result is public.
- Explicit forbidden actions: no passwords, seed phrases, private keys, deposits, KYC bypass, or payment.

## Funding

Use `test_no_payout` for controlled drills. For a paid task, keep it `awaiting_funding` until escrow/pool funding is recorded.

## Completion

A task is not complete because it was created or claimed. Require human evidence, a verification decision, and settlement only when funded.
