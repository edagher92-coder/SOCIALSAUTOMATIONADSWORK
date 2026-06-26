# SCHEDULE.md - Nightly Ad Review & Optimise loop (run from phone)
Purpose: every night, automatically check -> evaluate -> improve the live Snow Flow ads, then write a findings report and update MEMORY. Anything that spends money is queued for Elie's one-tap YES in the morning.

## How to run it
- From your phone (manual): open Cowork, ensure the Drive folder is connected, and send: "Run SCHEDULE.md".
- Automatic: ask once - "schedule SCHEDULE.md nightly at 9pm AEST" - Cowork runs it (still only reports money moves, never spends without YES).

## Prerequisites
1. Connected folder has this file + MEMORY.md.
2. Meta scripts reachable: meta_ads_api.py, meta_business_api.py + token files (gitignored, sandbox). Token permanent; OAuth 190 -> token-renewal note in META-API-RUNBOOK.md.
3. Never print the token. Account = act_179081790.

## The nightly steps
1. Load memory: read MEMORY.md.
2. Pull performance (READ - pre-approved): status; insights 1 (24h); insights 7. Capture per campaign/ad set: spend, reach, impressions, link clicks, messaging conversations/leads, CTR, CPC, cost per lead, frequency. Focus: Service 52587108383208 and 03 Warm 52586494783208.
3. Pull engagement + Sam: page-insights 1; posts 5. Check Reel comments for FREEZE leads; confirm Sam awake (GET snowflow-sam-bot.onrender.com -> ok:true).
4. Trends: today vs yesterday vs 7-day. Flag rising/falling cost per result, ad fatigue (freq >2.0 + falling CTR), winning vs dying creative.
5. Evaluate vs KPIs + kill-rule. Label each ad set: Scale / Hold / Refresh / Pause.
6. Decide actions. Auto-apply (pre-approved): none that spend; may pause a disapproved/broken ad. Queue for YES (money): budget changes/new spend with exact command + rationale; do NOT run until YES.
7. Write report -> reports/ad-review-YYYY-MM-DD.md.
8. Update memory: append dated one-liner to MEMORY.md.
9. Morning handoff: report + queued YES list.

## KPIs & 30-day kill-rule
| Metric | Target | Action if breached |
|---|---|---|
| Cost per conversation (Service) | <= ~$8 | >$12 for 3 days -> refresh creative or tighten targeting |
| CTR (all) | >= 1.0% | <0.6% for 3 days -> new hook/cover |
| Frequency (Warm) | <= 2.0/wk | >2.5 -> widen audience or trim Warm budget |
| Daily pacing | $11/day (Service $7 + Warm $4) | drift >20% -> flag |
| 30-day kill-rule | beat target by day 30 | underperformer -> pause, reallocate to winner |

## Guardrails
Budget change = Elie types YES first. Reads + status toggles pre-approved. Never DELETE (archive). No prices / no %-cheaper in creative. One CTA. Cut non-performers within 30 days. Never print the token. HARD CAP <= $400/mo total.
