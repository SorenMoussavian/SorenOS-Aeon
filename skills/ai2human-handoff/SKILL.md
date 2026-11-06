---
name: ai2human-handoff
description: Legacy combined AI2Human create-and-poll workflow. Use only for existing Aeon installations that already invoke this skill; for new work, classify with ai2human-route-task, use a specialist task skill, then track with ai2human-proof-lifecycle.
---

This is a compatibility skill for existing installations. For new work, start with `$ai2human-route-task`, invoke the selected specialist skill, then use `$ai2human-proof-lifecycle`.

AI2Human is the human execution layer for agents. This skill turns the current Aeon instance into a requester: it dispatches the blocked step to the AI2Human platform, where verified operators execute it and return structured proof. Settlement is gated by verification.

Canonical loop:

```text
task -> human execution -> proof -> verify -> settle
```

## When to use

Route work here when the next step needs:

- a real X/social account action (follow, like, repost, reply)
- screenshot, receipt, timestamp, image, or live URL proof
- local / physical-world confirmation (is the merchant open, did the event land)
- signature capture, physical pickup, or handoff
- human judgment, review, or explanation
- manual proof review before USDC settlement

Do **not** use this for fake reviews, spam, credential handling, KYC abuse, deposits, or actions that violate another platform's rules.

## Prerequisites (one-time, per installation)

1. Get a developer API key at <https://ai2human.io/developers/api-keys>.
2. Set the GitHub Actions secret on this Aeon project:
   - `AI2HUMAN_AGENT_KEY` — required for dispatch and poll.
3. Optional notification secrets (used by the poll script):
   - `TELEGRAM_BOT_TOKEN`, `TELEGRAM_CHAT_ID`, `DISCORD_WEBHOOK_URL`, `SLACK_WEBHOOK_URL`
4. Optional repo variable:
   - `AI2HUMAN_BASE_URL` — default `https://ai2human.io`

## What to do

### Dispatch a blocked step

Build an intent from the workflow context. Never invent `fundingMode`, `rewardUsdc`, `deadlineHours`, or proof requirements — they must come from the requester or workflow.

```json
{
  "intentId": "unique-slug-001",
  "title": "Verify Merchant X is open at 123 Main St",
  "description": "Visit the location, confirm the merchant is operating, and submit photo evidence with a timestamp.",
  "category": "local_verification",
  "fundingMode": "test_no_payout",
  "rewardUsdc": 5,
  "deadlineHours": 48,
  "proofRequirements": ["photo", "timestamp"],
  "location": "123 Main St",
  "agentName": "Aeon",
  "dryRun": false
}
```

`fundingMode` is mandatory:

- `test_no_payout` creates an explicitly unpaid controlled task and must not expose a claimable reward.
- `funded` creates a paid-task draft in `awaiting_funding`; it is not live or claimable until funding is recorded.

The numeric `rewardUsdc` for `test_no_payout` is planning metadata required by the current API contract, not an available reward.

Then run the bundled script with the intent inline:

```bash
node skills/ai2human-handoff/scripts/ai2human-handoff-create.mjs --direct '<intent-json>'
```

Supported `category` values:

`local_verification`, `identity_action`, `physical_task`, `digital_task`, `compliance_check`, `errand`

Supported `proofRequirements` values:

`screenshot`, `photo`, `url`, `timestamp`, `receipt`, `signature`, `notes`

`location` is required for `local_verification`, `physical_task`, and `errand`.

Natural tasks use `platform: real_world` and `custom-task-spec/v1`. Never route them through X or Telegram campaign templates. Social actions belong only in `$ai2human-social-campaign` when explicitly requested.

On success the script prints the `task_id` and `task_url`. Record them in the run summary and in the daily memory log.

### Safety defaults

1. **Dry-run by default.** An intent dispatches only with `"dryRun": false`. Set the `AI2HUMAN_DRY_RUN=true` env var to force dry-run for everything.
2. **Idempotent.** `intentId` is recorded in `.ai2human-state/tasks.json`; repeated dispatches never create duplicates.
3. **No destructive paths.** The skill never cancels, deletes, or refunds tasks.

### Watch the task (poll)

The bundled poll script watches open tasks and notifies only on transitions (proof submitted / verified / settled / expired):

```bash
node skills/ai2human-handoff/scripts/ai2human-handoff-poll.mjs
```

The operator should run it on a schedule (cron or GitHub Actions, e.g. `*/15 * * * *`). State persists in `.ai2human-state/tasks.json` at the repo root, so repeated runs do not re-notify.

## Output

End with a `## Summary` block: intent dispatched, `task_id`, `task_url`, dry-run yes/no, and the next expected transition (proof submitted).
