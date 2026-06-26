# SKILL — Boost a Snow Flow post/reel (repeatable runbook)
Last run: 2026-06-27 — boosted FB freeze reel `995099763279840`, $10/day x 7d = A$70, NSW, goal=messages. PUBLISHED (in review).
Trigger phrase next time: **"Boost the latest reel, $X/day for Y days."**

---

## 0. WHY this runbook exists (the hard-won lesson)
The Meta **Marketing API is blocked for new ads** because the issuing app (`1194338482780762` "Claude API") is in **Developer mode**. Proven error codes:
- New **campaign** -> `Invalid parameter` subcode **4834011** (dev-mode apps can't create campaigns).
- New **ad creative** (video_data) -> subcode **1885183** "created by an app in development mode".
- Promote existing **reel** as an ad via API -> subcode **2061015** (reels not ad-eligible this way).
- WORKS via API in dev mode: **status toggles** (pause/activate/archive), **new ad SETS under an existing campaign**, reading insights, creating a creative from an existing post `object_story_id`.

=> **Paid NEW ads = use the Business Suite "Boost" browser flow** (Boost uses Meta's internal pipeline, not the restricted API).

## 1. THE THREE FACEBOOK DOMAINS
| Domain | Use it for |
|---|---|
| **business.facebook.com** (Meta Business Suite) | PRIMARY. Boost posts/reels, Content library, Ads Manager, Audiences, Inbox. |
| **developers.facebook.com** | App settings (flip app **614890192017160** or **1194338482780762** -> Live to unlock API ad creation), Graph API Explorer (mint tokens), app secrets. |
| **facebook.com** | The Page itself - organic posting, viewing the live reel, in-app Boost fallback. |

## 2. BOOST RUNBOOK (browser via Claude-in-Chrome) — exact click-path
1. Connect the work-laptop Chrome (logged into Snow Flow Sydney). [switch_browser -> user clicks Connect]
2. business.facebook.com -> **Content** (left nav) -> **Posts & reels -> Published**.
3. Find target post (newest freeze reel) -> click **Boost**.
4. **Goal:** keep **Get more messages** (feeds Sam). (Or engagement/video views for pure reach.)
5. **Ad text:** leave existing compliant caption (no price, "Comment FREEZE").
6. **Special Ad Category:** OFF. Leave "financial products" UNticked.
7. **Audience -> "People you choose through targeting" -> pencil (Edit audience):** Gender All, Age 18-65+, **Locations: remove Australia, add New South Wales (State)** -> Save audience -> Confirm.
8. **Schedule:** "Choose end date" -> Days = Y (e.g. 7).
9. **Daily budget:** pencil on $ amount -> set $X (e.g. 10).
10. **Budget sharing:** toggle **OFF**.
11. **Placements:** leave **Advantage+** (FB+Messenger+Instagram+Audience Network) ON.
12. **Payment:** PayPal on file.
13. Review Payment summary ($X/day x Y = total) -> **Publish** -> "Ad in review" = success.

## 3. GUARDRAILS (always)
- **Spend = Elie's explicit OK** on the $ before the final Publish. Pausing never needs approval.
- **Caps:** <= $13.15/day total, <= $400/month across ALL ads (Service already ~$7/day ~ $213/mo).
- **Compliance:** no prices, no "%-cheaper" in creative. Pricing only in the DM (Sam).
- **Geo:** Sales = NSW. Service & repairs = Sydney/Greater Sydney.
- **Circuit-breakers / change-freeze:** after launch don't touch for **4 days**; auto-pause if cost/lead >$15 (2d), CPC >$1.50 (2d), pace >$13/day. (GUARDRAILS-AND-FIX-PLAN.md.)
- **Never** click "Reset amount spent" (clears account spend-limit safety) without Elie's OK.
- **Never** log in / enter credentials. Boost runs in Elie's already-logged-in Chrome.

## 4. FULL AUTO option
Flip an app to **Live** at developers.facebook.com (app **614890192017160** "Comments" has Business access, or **1194338482780762**): add Privacy Policy URL + category -> toggle Live (may need Business Verification). Once Live, the Marketing API can create campaigns + creatives directly -> launch fully via script (still paused -> your YES). Until then, use the browser Boost runbook.

## 5. Honest limit
Meta requires a human moment for ad payment/standards, so the **final Publish (spend) stays a confirm step**. Everything up to it is automated by following this runbook.
