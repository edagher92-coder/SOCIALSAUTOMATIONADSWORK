# AdPilot integration handoff — Ad Command Center + Snow Flow test account
From: Cowork (Elie's Snow Flow session) · 2026-06-26 · For: Claude Code / AdPilot dev team

## Ask
1. **Integrate the "Ad Command Center" dashboard into AdPilot** (target the top-2 plans).
2. **Set Snow Flow Sydney as the pilot/TEST account** for AdPilot.

## Ready component (proven, working today)
- File: `SnowFlow-Ad-Dashboard.html` (this folder) — self-contained, light-mode, animated.
- Shows: KPIs (spend, leads, cost/lead, impressions, video views), account spend-cap gauge, cost-per-lead (Warm vs cold), campaign cards, funnel, ads table, hero Reel links.
- Data: Meta Marketing API. Currently seeded/inlined. In-app it should call AdPilot's backend, not hold a token client-side.

## Test account facts (Snow Flow Sydney)
- Ad account act_179081790 (AUD). Page 100905889356891. IG 17841407475691689 (@snowflowsydney).
- Live campaigns: 08 Service 52587108235008 (adset 52587108383208, $7/day) | 03 Warm 52586494783208.
- Account spend cap = $400 (~$336 used). Monthly cap rule: <= $400.
- Auth = never-expiring Page token from app 1194338482780762 (server-side only, gitignored). App is in DEV mode -> must be Live to create ad creatives via API.

## Integration requirements
- **Backend-mediated data**: AdPilot server pulls Meta insights and serves JSON; never expose tokens in the browser.
- **Refresh cadence (Elie's spec): front-end every 60s** (show live "updated Xs ago") backed by a **~5-minute server-side cache** of the Meta pull (Meta insights only update every few min; 60s UI + 5min source = live feel, accurate, no rate-limit throttling). Manual refresh + on-load too.
- Reuse proven pieces: meta_ads_api.py / meta_business_api.py (status/insights/budget), SCHEDULE.md nightly scoring, compliance guardrails (no price / no %-cheaper / one CTA), Sam comment->DM engine.
- Guardrails to preserve: human-in-the-loop for spend + publish; launch paused first; never commit secrets; cut non-performers in 30 days.

## Approval gate
Route through 2 manager approvers (Product + Engineering) + one overall rating. Only GO items ship.
