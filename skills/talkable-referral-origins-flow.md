---
name: Register an off-site purchase or event to close a referral
description: Feed a backend/off-site purchase or CRM event to Talkable's Origins API so a referral chain closes and the advocate is rewarded, then approve or void the referral.
api: openapi/talkable-v2-openapi-original.yml
operations: [createOrigin, createPurchase, createEvent, updateReferralStatus, markOriginAsRefunded]
---

# Register an off-site purchase or event to close a referral

Talkable's standard front-end integration captures on-site purchases automatically.
Use the **Origins API** to feed backend, subscription, in-store, or approval events
so Talkable can attribute the referral and reward the advocate.

## Auth & conventions
- Base URL: `https://www.talkable.com/api/v2`
- Auth: `Authorization: Bearer <API_KEY>` (Site Settings -> API Integration -> API Key).
- Every request is site-scoped: include `site_slug`.
- Responses use the envelope `{ "ok": true, "result": ... }`; errors return `{ "ok": false, "error_message": "..." }` with a standard HTTP status (400/401/404/422).
- No idempotency-key header is documented — de-duplicate on your side before submitting.

## Steps
1. Capture the Talkable `visitor_uuid` from the Friend's destination URL (`tkbl_cvuuid`) when they arrive from a share link, and store it.
2. When the Friend completes a qualifying action, submit the origin:
   - Purchase: `POST /api/v2/origins/purchase` (**createPurchase**), or
   - Event/appointment/approval: `POST /api/v2/origins/event` (**createEvent**), or
   - Generic origin: `POST /api/v2/origins` (**createOrigin**).
   Include the stored `uuid` when available so Talkable links the origin to the advocate's offer.
3. To control payout, set the referral state with `PUT /api/v2/origins/{origin_slug}/referral` (**updateReferralStatus**) using `{"status":"approved"}` or `{"status":"voided"}`.
4. If the underlying purchase is later returned/canceled, call `POST /api/v2/origins/{origin_slug}/refund` (**markOriginAsRefunded**) to reverse it.

## Notes
- Call the referral-status endpoints for all orders; Talkable decides which are tied to referrals.
- For high volume use the batch endpoints (`/api/v2/origins/batch/purchases`, `/batch/events`).
