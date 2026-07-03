# Snow Flow — Socials & Ads Automation

Working repo for Snow Flow Sydney's paid + organic ad automation: the SFX3 “freeze” Reel campaign, Meta ads management, the Sam comment→DM auto-reply bot, and a live Ad Command Center dashboard.

## Start here
- **`MEMORY.md`** — single source of truth (live state, IDs, compliance rules §0). Read first.
- **`START.md`** — resume bootstrap for a fresh session.

## Key files
- `SnowFlow-Ad-Dashboard.html` — live Meta ads dashboard (KPIs, spend-cap, funnel, cost/lead)
- `SCHEDULE.md` — nightly ad-review runbook (“Run SCHEDULE.md”)
- `3-Campaign-Plan-REVIEW.md` — Care Plan / Hire / EOFY plan
- `SFX3-Freeze-VIRAL-content-pack-v2.md` — compliant captions/hashtags per platform
- `VIDEO-BUILD-RECIPE.md` + `build_freeze_video.sh` + `AUDIO.md` — reproducible video pipeline
- `SIGNOFF.md` — pre-publish panel sign-off
- `AdPilot-INTEGRATION-dashboard-and-test-account.md` + `NOTE-for-Claude-Code-AdPilot-research.md` — AdPilot handoff

## Compliance (non-negotiable)
No prices in ad creative · never “%-cheaper” claims · one CTA · faceless real-machine visuals · CTA keyword FREEZE (sales) / SERVICE (service).

## Security
Meta tokens & app secrets are **gitignored and never committed**. Large video files live in Google Drive, linked from the docs.

## Skill router (`skill_router/`)

Vendored copy of the auto-registering skill router from the Snow Flow monorepo
(`edagher92-coder/Claude-code-`, `src/skill_router/` — the source of truth; fix
bugs there first, then re-copy). Stdlib-only, zero config: it reads every
`*/SKILL.md` under `.claude/skills/` (or a top-level `skills/`) as its routing
table and ranks the skills against a natural-language query.

```bash
python -m skill_router "which skill should I use"   # route a query
python -m skill_router --registry                   # list registered skills
python -m skill_router --index                      # regenerate the skills INDEX.md
```

Tests: `python -m pytest tests/test_skill_router.py` (stdlib + pytest only).
