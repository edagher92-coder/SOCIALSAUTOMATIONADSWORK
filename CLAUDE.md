# CLAUDE.md — repo context for future sessions

## What this repo is

Working repo for **Snow Flow Sydney / The Slushie Co**'s paid + organic social ads
automation: the SFX3 "freeze" Reel campaign (hero creative + viral content pack),
Meta (Facebook/Instagram) ads management via the Marketing API, the "Sam"
comment→DM auto-reply bot, a nightly ad-review runbook, and a live Ad Command
Center dashboard (`SnowFlow-Ad-Dashboard.html`). It is an operations workspace
for one specific campaign body of work, not a generic app.

**Read `MEMORY.md` first** — it is the single source of truth for live state
(campaign/ad-set IDs, budgets, compliance rules in §0, status board, next
actions) and supersedes any other snapshot.

## Non-negotiable compliance rules (see MEMORY.md §0 for full detail)

- No prices in ad creative; no "X% cheaper than competitors" claims (ACL risk).
  Pricing lives in the DM (Sam), never the ad.
- One CTA per ad. Faceless, real-machine visuals only.
- CTA keywords: **FREEZE** (sales/SFX3) · **SERVICE** (service & repair).
- Sales = all NSW. Service & repairs = Sydney + Greater Sydney only.
- Contact: `Sydney@snowflow.com.au` · 0450 878 787. Never expose a personal email;
  never paste the Meta token into chat/docs (gitignored, sandbox-only).
- Any spend/budget change needs Elie's **typed YES** first. Reads and status
  toggles (pause/activate/archive) are pre-approved. Never DELETE a campaign
  (archive instead). Hard cap ≤ $400/month total ad spend.

## Index of key docs

- `README.md` — repo overview + entry points.
- `MEMORY.md` — live working memory: compliance (§0), brands, locked decisions,
  status board, key Meta IDs, guardrails, next actions. Read first, keep current.
- `SCHEDULE.md` — nightly ad-review runbook ("Run SCHEDULE.md"): pull insights →
  score vs KPIs + 30-day kill-rule → report → queue any spend for typed YES.
- `SIGNOFF.md` — pre-publish panel sign-off for the SFX3 "Freeze" Reel
  (creative + compliance scores, conditions, go-live status).
- `SKILL-boost-a-post.md` — repeatable runbook for boosting a post/reel via the
  Meta Business Suite browser flow (the Marketing API is blocked for new ads
  while the issuing app is in developer mode).
- `Service-WARM-and-TOFU-ad-copy-v3.md` — compliant ready-to-ship ad copy for
  the warm-retargeting + freeze-TOFU funnel rebuild.
- `VIDEO-BUILD-RECIPE.md` — the 6-stage recipe behind the freeze Reel (vertical
  reformat, speed-ramp, depth-of-field, captions, audio, mux), companion to
  `build_freeze_video.sh`.
- `AUDIO.md` — the CC0 audio track used in the freeze video, how it was edited
  in, and how to swap it.
- `3-Campaign-Plan-REVIEW.md` — the draft 3-campaign plan (Care Plan / Event
  Hire / EOFY sales) with budget split, KPIs, and launch sequence. Not
  auto-launched — for review.
- `SFX3-Freeze-VIRAL-content-pack-v2.md` — compliant captions/hashtags per
  platform (IG/FB/TikTok) for the SFX3 freeze Reel.
- `AdPilot-INTEGRATION-dashboard-and-test-account.md` — handoff asking to
  integrate the Ad Command Center dashboard into AdPilot and use Snow Flow
  Sydney as AdPilot's pilot/test account.
- `NOTE-for-Claude-Code-AdPilot-research.md` — companion handoff launching a
  research initiative to turn this project's proven components into AdPilot
  plan-tier features.
- `GUARDRAILS-AND-FIX-PLAN.md` — incident writeup (the "443% cost spike") and
  the resulting guardrails: change-freeze, circuit-breakers, one-driver rule.

## skill_router/ (vendored copy)

`skill_router/` is a **vendored copy** of the auto-registering skill router.
The source of truth is `edagher92-coder/Claude-code-` → `src/skill_router/` —
**fix bugs there first, then re-copy here.** It reads every `*/SKILL.md` under
`.claude/skills/` and ranks them against a natural-language query (stdlib-only,
no external deps).

```bash
python -m skill_router "which skill should I use"   # route a query
python -m skill_router --registry                   # list registered skills
python -m skill_router --index                       # regenerate .claude/skills/INDEX.md
```

## .claude/skills

Skills currently registered in `.claude/skills/` (see `INDEX.md` for the
generated registry): `skill-router`, `auto-escalate`, `balanced`, `deep`,
`quick`. `.claude/settings.json` allowlists the `Skill` tool, sets `model:
sonnet` with `fallbackModel: haiku`, and enables the same 19-plugin family
used across the other Snow Flow repos.

## Workflow

- Tests: `python -m pytest tests/test_skill_router.py` (stdlib + pytest only,
  covers the vendored router).
- Branch convention: `claude/<slug>` feature branches, PR'd to `main`.
- Never commit Meta tokens/app secrets — they are gitignored; large video
  files live in Google Drive, linked from the docs.
