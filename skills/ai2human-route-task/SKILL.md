---
name: ai2human-route-task
description: Classify a blocked agent step into the correct AI2Human workflow before dispatch. Use when a request may need digital QA, local verification, a physical handoff, research/compliance review, or an explicit social campaign, and especially when funding mode or proof requirements are unclear.
---

# AI2Human Task Router

Route by the human action, not by the reward or platform where the request originated.

## Decision

1. **Digital QA** — browser/device reproduction, page availability, app behavior, screenshot-backed UI checks. Use `$ai2human-digital-qa`.
2. **Local verification** — a person must visit or observe a named place. Use `$ai2human-local-verification`.
3. **Physical handoff** — pickup, delivery, signature, counter code, or physical errand. Use `$ai2human-physical-handoff`.
4. **Research/compliance** — source access, document review, bounded judgment, or policy checklist. Use `$ai2human-research-compliance`.
5. **Social campaign** — only when the requester explicitly asks for a social action and supplies the exact public link/handle. Use `$ai2human-social-campaign`.
6. **Lifecycle only** — task already exists and the user asks about proof, verification, funding, payment, refund, or status. Use `$ai2human-proof-lifecycle`.

## Funding Gate

Choose exactly one:

- `funded`: proposed reward requires recorded escrow/pool funding before publication.
- `test_no_payout`: execution may be tested, but the UI and API must say no payout.

Never treat `reward_usdc` as proof of funding. Never publish an unfunded paid task as live.

## Output

Return: selected skill, category, funding mode, deadline, required proof, acceptance criteria, and missing requester inputs. Do not dispatch until these are explicit.
