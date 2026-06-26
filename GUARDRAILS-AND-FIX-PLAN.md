# Ad-Spend Incident + Fix Plan - the "443% cost spike"
Date 2026-06-27 - Status: ALL ADS PAUSED (0 active) - Cross-domain review by the AI Specialist team (Media buying, Data, Risk/Ops, Compliance, Automation).

## 1. What happened
Cost per result and CPC spiked ~+443%: good-day lead ~$3.75 vs recent ~$20; CPC $0.60 -> $2.38; CPM peaked $27.82. On $5-7/day that's a few real dollars, but the % is the early warning.

## 2. Root causes - by domain
- Media buying: (a) LEARNING-PHASE RESET - the ad set was paused/activated/budget-changed/creative-swapped many times in one day (API + Ads-Manager drafts fighting). Each change throws Meta back into learning -> CPC/CPM spike. (b) Expensive creative carrying the account - cold "machine-down" ~$20/lead vs warm retargeting ~$3.17/lead.
- Data: no live alerting; tiny budgets magnify % swings.
- Risk/ops: no change-freeze, no spend circuit-breaker, two drivers (UI drafts + API) editing the same ad set.
- Compliance: clean (no price / no %-cheaper) - not a factor.

## 3. The fix - every domain
A. Stop the churn: ONE active creative. CHANGE-FREEZE - no status/budget/creative edits for 3-4 days after a change. One driver only (API or UI, never both).
B. Cheaper structure: WARM-FIRST (retargeting $3.17/lead). Freeze video = cheap top-of-funnel (video-views) feeding the retarget pool. Cold last/small.
C. Circuit-breakers (auto-STOP, no YES needed): hard caps $13/day + $400/mo. Auto-pause an ad set if cost/lead > $15 for 2 days; OR CPC > $1.50 for 2 days; OR daily pace > $13/day; OR frequency > 2.5. Nightly SCHEDULE.md runs the checks and pauses on breach, then reports.
D. Alerting: dashboard + nightly report flag any >30% day-over-day move in CPC / cost-per-lead / CPM + frequency.
E. Governance: spend/budget changes = Elie typed YES. Paused-first. No churn. Never log into accounts. Never commit secrets.

## 4. Corrected re-launch plan
1. Flip Meta app 1194338482780762 -> Live (unlocks compliant new creatives).
2. Rebuild WARM retargeting (no-price) = primary. Freeze video TOFU (video-views) builds the audience.
3. Launch PAUSED -> typed YES -> activate -> DO NOT TOUCH 4 days -> nightly auto-guard.
4. Stay within $400/mo; breakers armed from minute one.

## 5. Interim option (before app->Live)
Run ONE stable compliant ad (machine-down) at $5/day with auto-pause breaker (cost/lead > $15 for 2 days), untouched 4 days. Caveat: cold = pricier; warm rebuild is the real fix.
