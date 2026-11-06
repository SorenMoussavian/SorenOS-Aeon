# State Model

Workflow: needs_info, draft, awaiting_funding, live, in_progress, proof_review, revision_required, verified, settlement_submitted, settled, cancelled, expired, refund_pending, refunded, disputed, needs_review.

Funding: none, not_required, funding_pending, funding_submitted, funded, failed, refund_pending, partially_refunded, refunded, cancelled, needs_review.

Settlement: not_ready, ready, settlement_submitted, settled, failed, cancelled, needs_review.

`test_no_payout` uses funding `not_required` and must never expose a reward claim.
