# Kredivo

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Kredivo is a Southeast Asian consumer credit platform operated by FinAccel (Kredivo Group), best known for its "Buy Now, Pay Later" product offering instant real-time credit decisioning for online and offline purchases across Indonesia, Vietnam, Thailand and the Philippines. For developers, Kredivo publishes the Kredivo Checkout API — a merchant-facing HTTP/JSON payment API that lets an ecommerce store, marketplace, POS or EDC terminal offer installment payment (30 days through 24 months) at checkout. The API covers 2-click redirect checkout, 0-click tokenized express checkout, dynamic and static QR checkout for physical stores, EDC terminal checkout, transaction status polling, full and partial cancellation, user credit-limit lookup, and an installment/interest calculator, with asynchronous push notifications to a merchant-supplied push_uri and a signature-key confirmation callback. Kredivo Group also operates KrediFazz (instant cash loans), Krom (digital bank), Timo and GajiGesa.

**Areas:** Company · Payments · Buy Now Pay Later · Lending · Consumer Credit · Checkout · Fintech · E-Commerce · Indonesia · Southeast Asia · Financial Services

## Kredivo on APIs.io

Everything profiled for Kredivo is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/kredivo/](https://apis.io/providers/kredivo/)** — or go straight to one of the APIs or properties below.

### APIs (5)

- [Kredivo Calculator API](https://apis.io/apis/kredivo/kredivo-calculator-api/) — Installment, interest and service-fee breakdown.
- [Kredivo Checkout API](https://apis.io/apis/kredivo/kredivo-checkout-api/) — Initiating a Kredivo installment checkout (2-click, 0-click, QR, EDC).
- [Kredivo Confirmation API](https://apis.io/apis/kredivo/kredivo-confirmation-api/) — Confirming a transaction after a push notification.
- [Kredivo Tokenization API](https://apis.io/apis/kredivo/kredivo-tokenization-api/) — Managing tokenized (0-click) shoppers and their credit limits.
- [Kredivo Transactions API](https://apis.io/apis/kredivo/kredivo-transactions-api/) — Status, cancellation and reversal of transactions.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Kredivo Checkout Webhooks](https://apis.io/asyncapis/kredivo/kredivo-checkout-webhooks/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [kredivo](https://apis.io/servers/kredivo/kredivo/)

#### Collections (5)

Postman and Open Collection request collections.

- [Kredivo Checkout Calculator API](https://apis.io/collections/kredivo/open-kredivo-calculator-api/)
- [Kredivo Calculator Checkout API](https://apis.io/collections/kredivo/open-kredivo-checkout-api/)
- [Kredivo Checkout Calculator Confirmation API](https://apis.io/collections/kredivo/open-kredivo-confirmation-api/)
- [Kredivo Checkout Calculator Tokenization API](https://apis.io/collections/kredivo/open-kredivo-tokenization-api/)
- [Kredivo Checkout Calculator Transactions API](https://apis.io/collections/kredivo/open-kredivo-transactions-api/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Kredivo Checkout Overlay](https://apis.io/overlays/kredivo/kredivo-checkout-overlay/)

#### Security (3)

Authentication, trust centre, and vulnerability disclosure.

- [Kredivo Authentication](https://apis.io/security/kredivo/kredivo-authentication/)
- [Kredivo Domain Security](https://apis.io/security/kredivo/kredivo-domain-security/)
- [Kredivo Vulnerability Disclosure](https://apis.io/security/kredivo/kredivo-vulnerability-disclosure/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://finaccel.co)
- [Documentation](https://doc.kredivo.com/)
- [Getting started](https://doc.kredivo.com/)
- [API reference](https://doc.kredivo.com/)
- [Sign up](https://kredivo.com/merchant/)
- [Support](https://kredivo.com/faqs/)
- [Blog](https://blog.kredivo.com/)
- [Terms of service](https://kredivo.com/p/in/tos.html)
- [Privacy policy](https://kredivo.com/p/in/privacy.html)
- [Status](https://status.kredivo.com/)

## API Evangelist rating

**Kin Score 40.4** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 78.6 |
| Contract quality | 22.0 |
| Contract governance | 4.5 |
| Operational transparency | 34.2 |
| Developer ergonomics | 73.2 |
| Access clarity | 34.2 |

**Agent readiness 35.0** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | documented (partial credit) |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | documented (partial credit) |
| Error Semantics | documented (partial credit) |
| OpenAPI Examples | verified |
| Rate Limit Signal | — |
| Event Surface Described | yes |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | na |
| Operation Distinctiveness | distinct |

**Regulatory alignment 27.7** — Payments

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/kredivo/](https://apis.io/providers/kredivo/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Kredivo: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/kredivo/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
