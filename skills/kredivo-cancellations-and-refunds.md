---
name: kredivo-cancellations-and-refunds
description: Cancel a Kredivo transaction in full or in part without tripping the idempotency guard, and reverse an incomplete offline EDC transaction — including how to compute the cancellable remainder and why an explicit cancellation_id is mandatory.
api: Kredivo Checkout API
spec: openapi/kredivo-checkout-openapi.yml
docs: https://doc.kredivo.com/
operations:
  - checkTransactionStatus
  - cancelTransaction
  - reverseOfflineTransaction
generated: '2026-07-19'
method: generated
source: openapi/kredivo-checkout-openapi.yml, https://doc.kredivo.com/
---

# Kredivo cancellations and reversals

Unwinds a Kredivo transaction — fully, partially, or as an offline reversal.

## Step 1 — Read current state first

Call `checkTransactionStatus` with `server_key` and `order_id` before cancelling. You need the
`transaction_id` (cancellation requires it) and the current `amount`.

If `transaction_status` is already `cancel`, stop — you are done.

## Step 2 — Compute the cancellable remainder

Take the transaction `amount` and subtract every cancellation you have already had accepted. That
remainder is your ceiling.

Exceeding it returns error code **2415**, `"Cancellation amount bigger than remaining transaction
amount that can be cancelled"`. Zero and negative amounts are rejected the same way. Kredivo exposes
no operation to list prior cancellations, so **you must track them yourself** — there is no way to
ask Kredivo what is left.

## Step 3 — Cancel

Call `cancelTransaction`. Required: `server_key`, `order_id`, `transaction_id`.

- **Full cancellation** — omit `cancellation_amount`, or pass exactly the transaction amount.
- **Partial cancellation** — pass an amount below the remainder.

Optional but recommended: `cancellation_reason`, `cancelled_by`, `cancellation_date` (epoch
seconds).

### Always send an explicit `cancellation_id`

This is the single most important rule in this flow.

`cancellation_id` (max 60 chars) is your idempotency key. One `order_id` can carry many of them,
which is exactly how multiple partial cancellations stay individually replayable.

If you omit it, Kredivo derives the uniqueness key from the pair
(`order_id`, `cancellation_amount`) and **rejects a repeat of that same pair within one hour**. That
means two legitimately distinct partial cancellations of the same value against the same order — two
identically priced line items refunded in the same session, a routine case — will silently fail the
second time.

Generate a `cancellation_id` from your own refund record id. Never reuse one for a different
cancellation.

## Step 4 — Interpret the result

Remember that **HTTP 200 is not success**. Read the body `status`.

- `status: OK` with `"Succes cancelling transaction by amount"` (Kredivo's spelling) — done.
- Code **1009**, `"Transaksi sudah dibatalkan."` — already cancelled. Terminal success. Do not retry.
- `"This cancellation_id is already PROCESSED."` — the idempotency guard firing. Terminal success,
  not a fault. Note that Kredivo does **not** replay the original success body, so your own record is
  the only account of what the first call did. Log it.
- Code **2415** — see Step 2.

Retrying blindly on any of these is wrong. All three are terminal.

## Offline (EDC) reversals

For an incomplete offline transaction, use `reverseOfflineTransaction` instead: `server_key` and
`order_id` only. It returns a bare `{status, message}`.

Reversal is a different operation from cancellation and carries **no idempotency key at all** — there
is no `cancellation_id` equivalent and no documented replay behaviour. Guard against double
submission on your side.

## Agent guidance

Every operation here moves money and none is reversible. Treat all of them as
escalation-required: confirm the amount and the target transaction with a human before calling. The
one safely autonomous step is `checkTransactionStatus` in Step 1.

## Related

- `errors/kredivo-error-codes.yml` — full error catalog including 2415 and 1009
- `conventions/kredivo-conventions.yml` — the idempotency model in detail
- `data-model/kredivo-data-model.yml` — why Cancellation is write-only
