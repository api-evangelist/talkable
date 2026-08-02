---
name: Look up and manage a customer (person) for support
description: Resolve a customer by slug, review their referrals/rewards/personal data, and run privacy actions (unsubscribe, anonymize) plus referral approval/voiding for support cases.
api: openapi/talkable-v2-openapi-original.yml
operations: [findPerson, getPersonalInformationAboutPerson, getRewardsReceivedByPerson, unsubscribePerson, anonymizePerson, updateReferralStatus]
---

# Look up and manage a customer (person) for support

Handle customer-support and privacy requests against a Talkable `person`.

## Auth & conventions
- Base URL: `https://www.talkable.com/api/v2`; auth `Authorization: Bearer <API_KEY>`; include `site_slug`.
- Envelope `{ "ok": true, "result": ... }`; errors `{ "ok": false, "error_message": "..." }`.

## Steps
1. Resolve the customer: `GET /api/v2/people/{person_slug}` (**findPerson**).
2. Review context as needed:
   - `GET /api/v2/people/{person_slug}/rewards` (**getRewardsReceivedByPerson**)
   - `GET /api/v2/people/{person_slug}/referrals_as_advocate` / `.../referrals_as_friend`
   - `GET /api/v2/people/{person_slug}/personal_data` (**getPersonalInformationAboutPerson**) for a GDPR/CCPA data request.
3. Privacy actions:
   - `POST /api/v2/people/{person_slug}/unsubscribe` (**unsubscribePerson**) to opt them out of emails.
   - `POST /api/v2/people/{person_slug}/anonymize` (**anonymizePerson**) to erase their PII (irreversible).
4. Resolve a disputed reward by setting the referral state via `PUT /api/v2/origins/{origin_slug}/referral` (**updateReferralStatus**) with `{"status":"approved"}` or `{"status":"voided"}`.

## Notes
- `anonymizePerson` is destructive — confirm the request is a verified erasure before calling.
- The same actions are exposed as MCP tools (find_person, manage_person, approve_or_void_referral) requiring the `mcp:write` scope and a CSP/Write role for writes.
