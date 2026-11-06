---
name: ai2human-local-verification
description: Create AI2Human location-bound verification tasks requiring a person to visit or observe a specified place and return current photo, timestamp, location context, price, stock, queue, opening-status, or signage evidence.
---

# AI2Human Local Verification

Use category `local_verification`. A concrete location is mandatory.

## Common Types

- Storefront hours or operating status.
- Shelf stock and visible price.
- Menu item and posted price.
- Venue queue and entry status.
- Local signage, event setup, or facility condition.

## Evidence

Require a current photo, server timestamp, short observation, and enough surrounding context to identify the requested place. Request GPS only when necessary; do not infer identity or precise location beyond the task.

## Acceptance

State what must be visible, how freshness is established, and what the evidence does not prove. Use the natural-task proof bundle and verifier, never an X/Telegram campaign template.
