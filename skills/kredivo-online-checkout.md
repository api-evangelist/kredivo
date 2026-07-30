---
name: kredivo-online-checkout
description: Take a shopper through a Kredivo installment ("Buy Now, Pay Later") checkout on an ecommerce site, from rendering the installment options to safely confirming that the transaction settled before fulfilling the order.
api: Kredivo Checkout API
spec: openapi/kredivo-checkout-openapi.yml
docs: https://doc.kredivo.com/
operations:
  - calculatePayments
  - createCheckoutUrl
  - confirmTransaction
  - checkTransactionStatus
generated: '2026-07-19'
method: generated
source: openapi/kredivo-checkout-openapi.yml, https://doc.kredivo.com/
---

# Kredivo online checkout (2-click)

Offers Kredivo installment payment on an ecommerce checkout and resolves the order to a
fulfilable state.

## Before you start

- You need the merchant `server_key`. It goes in the **JSON request body** of every call, not in a
  header. Sandbox and production keys are different, and they are indistinguishable by inspection —
  bind the key to the base URL in configuration.
- Sandbox is `https://sandbox.kredivo.com`. The production host is not published; request it from
  `merchops@kredivo.com`.
- Amounts are whole Indonesian rupiah. Requests send numbers; responses return decimal strings like
  `"1500100.00"`. Parse defensively.

## Step 1 — Show the installment options

Call `calculatePayments` with the cart total and line items to get each available tenure with its
interest rate, service fee and monthly installment. Render these before the shopper commits, so the
cost is visible up front.

If the shopper is already tokenized, pass their `user_token` — the options come back specific to
that shopper.

## Step 2 — Initiate the checkout

Call `createCheckoutUrl`. Required: `server_key`, `payment_type`, `transaction_details`
(with `order_id`, `amount`, `items`), `customer_details`, `push_uri`, `back_to_store_uri`.
`shipping_address` is required when shipping physical goods.

Rules that matter:

- **`order_id` is your idempotency key.** Re-submitting one Kredivo already processed returns
  `status: ERROR` with `"This order is already PROCESSED."` Generate a fresh `order_id` per attempt,
  or read the existing outcome with `checkTransactionStatus` instead of re-initiating.
- **`transaction_details.amount` must equal the sum of `items`.** Non-product amounts go in `items`
  under reserved ids: `shippingfee`, `adminfee`, `taxfee`, `discount`, `additionalfee`,
  `insurancefee`, `mixpayment`.
- Send `metadata` (`ip_address`, `user_agent`, `device_id`) — it feeds Kredivo's fraud detection and
  its absence makes a denial more likely.
- Set `expiration_time` (epoch seconds) if the 24-hour default is too long to hold inventory.
- For marketplaces, `sellers` is required, and each cart line attributes to a seller via
  `parent_type: "SELLER"` and `parent_id`.

Then redirect the shopper to the returned `redirect_url`.

## Step 3 — Check the response contract

**HTTP 200 does not mean success.** Read the body `status` field. On `ERROR`, read
`error.message` — `error.code` is frequently `null`, so message matching is often the only option.
Messages come back in both English and Indonesian.

## Step 4 — Receive the push notification

Kredivo POSTs the outcome to your `push_uri`. Respond with `{"status": "OK", "message": "..."}`.

**Do not fulfil on this payload.** It carries no signature header.

## Step 5 — Verify, then fulfil

Call `confirmTransaction` (`GET /kredivo/v2/update`) with the `transaction_id` and `signature_key`
from the notification. This is both the authenticity check and the authoritative read.

- `"Field secret_key is INCORRECT."` means the notification is unverified — do not fulfil.
- Fulfil **only** when `transaction_status` is `settlement`.
- `pending` is not a decline; the flow is still open. Keep waiting.
- `deny`, `expire`, `cancel` are terminal. Do not fulfil, and do not retry the same cart on a
  `fraud_status: deny` — an FDS denial is automatic.
- Never surface `fraud_status` or any denial reasoning to the shopper.

## Step 6 — Reconcile what never arrived

Kredivo publishes no webhook retry policy or delivery guarantee, so notifications can be lost. Run a
periodic sweep calling `checkTransactionStatus` by `order_id` for every order still unresolved after
a reasonable window. Treat this as required for correctness, not as an optimisation.

## Testing

Sandbox publishes a shared test shopper (username `81513114262`, password `663482`) and a fixed OTP
of `4567`. There is no published way to force a `deny`, `expire` or fraud decline in sandbox, and no
test clock — so the failure paths and the expiry behaviour cannot be reproduced on demand. Plan to
exercise those against production with small real amounts, carefully.

## Related

- `conventions/kredivo-conventions.yml` — auth, error and versioning semantics
- `errors/kredivo-decline-codes.yml` — what each terminal state means and what to do
- `asyncapi/kredivo-checkout-webhooks.yml` — the notification surface
