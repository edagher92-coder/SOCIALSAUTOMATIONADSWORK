# NOTE FOR CLAUDE CODE - launch: AdPilot plan-tier feature research & approval
From: Cowork (Elie's Snowflow session) - 2026-06-26 - For: Claude Code (dev agent)

## Mission
Launch a cross-functional research + design initiative - GUI/UX, Engineering, Integration - to define and get approval for new AdPilot features. Two goals:
1. Strengthen the top 2 plans (make them clearly worth it).
2. Add meaningful, significant features to the highest tier.
End state: an approved feature set + UX mockups + technical/integration design + phased build plan, signed off by 2 manager approvers (Product + Engineering) in parallel with one overall rating.

## Ground yourself first
- Locate the AdPilot repo + its current plan tiers, pricing, feature gating, and stack.
- Read this folder for proven, working components (below).

## Proven components from this project (candidate AdPilot features)
- Direct Meta Marketing API control (status/insights/pause/activate/budget), popup-free (meta_*_api.py + META-API-RUNBOOK.md).
- Comment->DM lead engine + "Sam" multi-channel auto-responder (Messenger/IG/WhatsApp).
- Nightly auto-review loop (SCHEDULE.md): pull insights -> score vs KPIs + 30-day kill-rule -> report -> queue spend for human YES.
- Auto creative pipeline (build_freeze_video.sh / VIDEO-BUILD-RECIPE.md): raw clip -> 9:16, grade, speed-ramp, depth-of-field, premium captions, CC0 audio.
- Live Ad Command Center dashboard (SnowFlow-Ad-Dashboard.html).
- Compliant copy generator (no price / no %-cheaper, CTA keyword).

## Research tracks (parallel)
1. GUI/UX - flows + mockups: campaign dashboard, creative/video studio, nightly-report & approval inbox, tier-gating UI, onboarding.
2. Engineering - feasibility, architecture, build phases, effort/risk; reuse proven components.
3. Integration - Meta/Google/TikTok ad APIs, CRM (ActiveCampaign/HubSpot), Xero, webhooks, OAuth + multi-account/white-label, ad-safe audio.

## Highest-tier differentiators to evaluate (pick meaningful, score willingness-to-pay)
Autonomous nightly optimisation w/ guardrails - AI creative + video auto-build at scale - predictive budget/ROAS - multi-account/white-label - advanced audience research - compliance engine - CRM nurture automation - priority support/SLAs/audit logs.

## Approval gate
2 manager approvers (Product + Engineering) review in parallel -> each score /10 + conditions -> one overall rating + GO / GO-WITH-FIXES / NO-GO. Only GO enters AdPilot. Keep an audit trail. Mirror guardrails: human-in-the-loop for spend/publish; never commit secrets.
