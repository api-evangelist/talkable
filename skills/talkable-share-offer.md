---
name: Share an advocate offer via email or social channel
description: Retrieve an advocate's referral offer and distribute it through email or a social channel, or share-and-claim in one call, as part of an API-driven referral program.
api: openapi/talkable-v2-openapi-original.yml
operations: [findAdvocateOffer, checkOfferClaimLinkStatus, shareViaEmail, shareViaSocialChannel, shareAndClaimOffer]
---

# Share an advocate offer via email or social channel

Drive the advocate share step of a referral program directly through the API.

## Auth & conventions
- Base URL: `https://www.talkable.com/api/v2`; auth `Authorization: Bearer <API_KEY>`; include `site_slug`.
- Envelope `{ "ok": true, "result": ... }`; errors `{ "ok": false, "error_message": "..." }`.

## Steps
1. Get the advocate's offer: `GET /api/v2/offers/{short_url_code}` (**findAdvocateOffer**). Read the `claim_urls` for per-channel share links.
2. Share it:
   - Email: `POST /api/v2/offers/{short_url_code}/shares/email` (**shareViaEmail**) with recipient details.
   - Social: `POST /api/v2/offers/{short_url_code}/shares/social` (**shareViaSocialChannel**) for a specific channel.
3. To create and claim in one step (e.g. a landing page you host), use `POST /api/v2/offer_claims` (**shareAndClaimOffer**).
4. Before redirecting a Friend, confirm the link is valid with `GET /api/v2/offers/{short_url_code}/claim_url_status` (**checkOfferClaimLinkStatus**).

## Notes
- For accounts created after April 2025, API requests auto-generate an Activity, so shares count toward advocate impressions.
