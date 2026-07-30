---
name: kredivo-tokenized-express-checkout
description: Tokenize a Kredivo shopper for 0-click express checkout, then place repeat purchases against the stored token — including checking remaining credit first, falling back to 2-click cleanly, and handling shopper-initiated deactivation.
api: Kredivo Checkout API
spec: openapi/kredivo-checkout-openapi.yml
docs: https://doc.kredivo.com/
operations:
  - createCheckoutUrl
  - confirmTransaction
  - getUserCreditDetails
  - deactivateUserToken
generated: '2026-07-19'
method: generated
source: openapi/kredivo-checkout-openapi.yml, https://doc.kredivo.com/
---

# Kredivo tokenized (0-click) express checkout

Turns a first Kredivo purchase into a stored token, then uses that token for one-step repeat
purchases.

## Step 1 — Tokenize on a normal checkout

Call `createCheckoutUrl` as you would for a 2-click checkout, plus:

- `tokenize_user: true`
- `client_user_key` — your own identifier for the shopper (required whenever `tokenize_user` is
  true). Published examples use an email address; any stable merchant-side identifier works.

The shopper completes the normal phone + PIN + OTP flow. The `user_token` comes back to you on the
**push notification payload**, retrieved through `confirmTransaction` — not in the synchronous
response to `createCheckoutUrl`.

Store the `user_token` against your `client_user_key`. Treat it as a credential: it authorizes
credit purchases for that shopper.

## Step 2 — Check remaining credit before offering 0-click

Before rendering Kredivo as a one-click option, call `getUserCreditDetails` with the `user_token`.

You get back `account_status`, `account_type` and `credit_limit_details[]` — the remaining limit per
tenure. Hide any tenure whose `remaining_user_limit` is below the cart total.

This is the whole point of the operation: it converts a hard decline into a quiet non-offer. A
shopper who is shown Kredivo and then denied is a worse outcome than one who was never shown it.

Note that `account_status` and `account_type` are integers whose value vocabularies Kredivo does not
publish. Do not branch on them without confirming their meaning with `merchops@finaccel.co`.

## Step 3 — Place the 0-click purchase

Call `createCheckoutUrl` with `user_token` set, `tokenize_user: false`, and the same
`client_user_key`. Everything else is a normal checkout body.

**Always handle `redirect_url` even on the 0-click path.** If the token is invalid or has been
deactivated, Kredivo does not hard-fail — it returns a `redirect_url` so you can fall back to a
regular 2-click checkout. Sending the shopper there is the correct recovery. Treating the response
as a failure instead is the most common way to break this flow.

The outcome still arrives asynchronously at `push_uri`, and you still must verify with
`confirmTransaction` and fulfil only on `transaction_status: settlement`. Tokenization removes shopper
friction, not the confirmation round trip.

## Step 4 — Handle deactivation from both directions

**Merchant-initiated:** call `deactivateUserToken` with `server_key` and `user_token`. Success
returns `is_active: false`. Do this whenever the shopper removes the payment method or closes their
account.

**Shopper-initiated:** a shopper can deactivate their tokenized account inside the Kredivo app.
Kredivo then pushes a notification carrying `message` and `client_user_key` to an endpoint you
register with Kredivo **out of band** — unlike `push_uri`, this destination cannot be set per
transaction. Contact Kredivo to register it.

On receipt, delete the stored `user_token` for that `client_user_key` and respond
`{"status": "OK"}`. If you skip this, the next 0-click attempt degrades to a 2-click redirect rather
than failing outright — recoverable, but a worse checkout than necessary.

Either way, `"Invalid user_token"` from `getUserCreditDetails` or `deactivateUserToken` means the
token is dead: discard it and re-tokenize on the shopper's next purchase.

## Security note

A stored `user_token` plus your `server_key` is enough to create a credit obligation for a real
consumer. Kredivo issues no scoped or reduced-privilege credential, so there is no way to grant a
subsystem read-only access to this surface. Store tokens encrypted, scope access tightly, and never
expose either value to a browser or an agent context.

## Related

- `skills/kredivo-online-checkout.md` — the 2-click flow this builds on and falls back to
- `conventions/kredivo-conventions.yml` — the body-carried credential model
- `data-model/kredivo-data-model.yml` — how User, CreditProfile and Transaction relate
