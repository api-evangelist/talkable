---
name: Manage a loyalty member's points balance
description: Find a Talkable loyalty member, inspect their collect/redeem history, adjust their points balance, and record redemptions.
api: openapi/talkable-v2-openapi-original.yml
operations: [findLoyaltyMember, updateLoyaltyMember, listCollectActions, createCollectAction, listManualAdjustmentActions, adjustLoyaltyMembersPointsBalance, listRedeemActions, createRedeemAction, listRedemptionBatches]
---

# Manage a loyalty member's points balance

Operate Talkable's loyalty program for a single member, keyed by email.

## Auth & conventions
- Base URL: `https://www.talkable.com/api/v2`; auth `Authorization: Bearer <API_KEY>`; include `site_slug`.
- Envelope `{ "ok": true, "result": ... }`; errors `{ "ok": false, "error_message": "..." }` (400/422 on validation).

## Steps
1. Resolve the member: `GET /api/v2/loyalty/members/{email}` (**findLoyaltyMember**).
2. Review activity:
   - `GET /api/v2/loyalty/members/{email}/collect_actions` (**listCollectActions**)
   - `GET /api/v2/loyalty/members/{email}/redeem_actions` (**listRedeemActions**)
   - `GET /api/v2/loyalty/members/{email}/manual_adjustment_actions` (**listManualAdjustmentActions**)
3. Award points for an action: `POST /api/v2/loyalty/members/{email}/collect_actions` (**createCollectAction**).
4. Manually correct a balance: `POST /api/v2/loyalty/members/{email}/manual_adjustment_actions` (**adjustLoyaltyMembersPointsBalance**).
5. Record a redemption: `POST /api/v2/loyalty/members/{email}/redeem_actions` (**createRedeemAction**); review batches with `GET .../redemption_batches` (**listRedemptionBatches**).
6. Update member attributes with `PUT /api/v2/loyalty/members/{email}` (**updateLoyaltyMember**).
