# Application Report — Snow Flow "Socials & Ads Automation"

**Prepared:** 2026-07-07 · **Repo:** `edagher92-coder/SOCIALSAUTOMATIONADSWORK`
**Scope:** What this application *is*, the use case it serves, how the pieces fit together, current live state, and the risks/gaps worth knowing.

---

## 1. Executive summary

This repository is the **operating system for a small business's paid-and-organic social advertising**, run largely by an AI agent (Cowork / Claude) with a human owner (Elie) holding the money and publish switches. It is not a conventional software product with a single binary; it is a **runbook-driven automation package** — a set of Markdown "programs," reproducible shell/Python tooling, a live HTML dashboard, and a hosted chatbot — that together let one operator plan, produce, launch, monitor, and de-risk Meta (Facebook/Instagram) ad campaigns for two related brands.

The whole system is engineered around one hard-won lesson: **on tiny ad budgets, uncontrolled changes and compliance mistakes are the real threat, not lack of features.** So the dominant design theme is *guardrails* — spend caps, change-freezes, circuit-breakers, compliance rules, and human-in-the-loop money gates.

**Two audiences / two lives for this repo:**
1. **Operational** — the day-to-day command center for Snow Flow's own ads (live today).
2. **Product seed** — a proven-components library being handed off to become **"AdPilot,"** a productized ad-automation SaaS, with Snow Flow as the pilot/test account.

---

## 2. The business being served

| Brand | What it sells | Geography |
|---|---|---|
| **Snow Flow Sydney** | Commercial slushie machine **SALES**, **SERVICE/REPAIR**, syrups & consumables. Australian-owned *manufacturer* (since 2007), in-house parts/service/warranty. | Sales = all **NSW**. Service & repair = **Sydney + Greater Sydney** only. |
| **The Slushie Co Sydney** | Event **HIRE** (slushy, soft serve, choc fountain, fairy floss, popcorn, hot choc). Revesby warehouse; Adelaide delivery. | NSW (Sydney metro) + Adelaide delivery. |

Contact of record: `Sydney@snowflow.com.au` · 0450 878 787. The owner's personal email is deliberately never exposed in creative.

---

## 3. Primary use case

**"Run a compliant, budget-safe lead-generation ad operation on Meta for a niche manufacturer, mostly hands-off, with an AI agent doing the work and a human approving spend."**

The funnel the whole system is built to drive:

```
Hero "freeze" Reel (broad, cheap, video-views)
        │  builds a viewer / engager pool
        ▼
WARM retargeting ad (re-engage that pool)
        │  one CTA → comment/DM a keyword
        ▼
FREEZE (sales) / SERVICE (service) keyword
        ▼
"Sam" auto-DM bot  →  captures lead, sends pricing in the DM (never in the ad)
        ▼
(planned) ActiveCampaign 3-email nurture
```

Economics that justify the funnel: **warm retargeting ≈ $3.17/lead vs cold ≈ $20/lead.** The cheap top-of-funnel video exists purely to fill the warm pool that closes cheaply.

---

## 4. What the application is made of

The repo is a collection of interoperating "modules," most expressed as executable runbooks rather than compiled code.

### 4.1 Knowledge / control plane
- **`MEMORY.md`** — the single source of truth: live state, all IDs, budgets, decisions, and the non-negotiable compliance rules (§0). Every automated session reads this first.
- **`README.md`** / `START.md` (bootstrap) — orientation and session resume.
- **`GUARDRAILS-AND-FIX-PLAN.md`** — the post-mortem + control system born from the "443% cost spike" incident.

### 4.2 Campaign strategy
- **`3-Campaign-Plan-REVIEW.md`** — three-campaign plan (Care Plan/Service, Event Hire, EOFY/Sales) with budget splits, KPIs, launch sequence.
- **`Service-WARM-and-TOFU-ad-copy-v3.md`** — ready-to-ship, compliance-checked ad copy for the warm-first funnel.

### 4.3 Creative production pipeline
- **`VIDEO-BUILD-RECIPE.md`** + `build_freeze_video.sh` + **`AUDIO.md`** — a reproducible, zero-paid-tools ffmpeg pipeline: raw phone clip → 9:16, colour-grade, speed-ramp, depth-of-field, Anton-font captions, CC0 audio normalised to −16 LUFS → master `SFX3-freeze-master_9x16.mp4`. Fully re-runnable; change a Drive ID or track URL and rebuild.
- **`SFX3-Freeze-VIRAL-content-pack-v2.md`** — platform-specific captions/hashtags/hooks (IG, FB, TikTok), all compliant.

### 4.4 Launch & operations runbooks
- **`SKILL-boost-a-post.md`** — the exact browser click-path to boost a post via Meta Business Suite. Exists because the **Marketing API is blocked for *creating* new ads** while the issuing app is in Developer mode (documented error subcodes 4834011 / 1885183 / 2061015). The API still works for status toggles, reads, and insights.
- **`SCHEDULE.md`** — the nightly auto-review loop: pull 24h/7d insights → score vs KPIs and the 30-day kill-rule → label each ad set Scale/Hold/Refresh/Pause → auto-pause on breach (no approval) → write a dated report → queue any spend for the owner's one-tap YES.
- **`SIGNOFF.md`** — the pre-publish review gate (Creative 7.5/10 + Compliance 8.5/10 → APPROVED).

### 4.5 The lead-capture bot
- **"Sam"** — a hosted (Render) comment→DM auto-responder across Messenger + Instagram (WhatsApp planned). Webhook-subscribed, 22 reply rules. Endpoint: `https://snowflow-sam-bot.onrender.com`. Note: on Render's free plan it sleeps when idle — a Starter upgrade ($7/mo) is recommended.

### 4.6 The dashboard
- **`SnowFlow-Ad-Dashboard.html`** — a self-contained "Ad Command Center": KPIs (spend, leads, cost/lead, impressions, video views), spend-cap gauge, warm-vs-cold cost-per-lead, campaign cards, funnel, ads table, hero-Reel links. Designed to be backend-mediated (server pulls Meta insights; tokens never in the browser) with a 60s UI refresh over a ~5-minute server cache.

### 4.7 Utility: skill router (`skill_router/`)
- A vendored, stdlib-only Python tool that reads every `*/SKILL.md` under `.claude/skills/` and ranks skills against a natural-language query. Zero-config, with a pytest suite. Source of truth lives in the Snow Flow monorepo; this is a maintained copy. It is **infrastructure for the AI operator**, not part of the ad product.

### 4.8 Productization handoff
- **`AdPilot-INTEGRATION-dashboard-and-test-account.md`** and **`NOTE-for-Claude-Code-AdPilot-research.md`** — the brief to turn these proven components into **AdPilot**, a tiered SaaS: strengthen the top two plans, add high-tier differentiators (autonomous nightly optimisation, AI creative/video at scale, predictive budget/ROAS, multi-account/white-label, compliance engine, CRM nurture), gated behind a 2-approver GO/NO-GO.

---

## 5. Architecture at a glance

```
┌───────────────────────────────────────────────────────────┐
│  HUMAN (Elie): owns money + publish switches; types "YES"  │
└───────────────┬───────────────────────────────────────────┘
                │ approvals
┌───────────────▼───────────────────────────────────────────┐
│  AI OPERATOR (Cowork/Claude)                                │
│  reads MEMORY.md → runs runbooks → obeys guardrails         │
└──┬─────────────┬───────────────┬──────────────┬────────────┘
   │             │               │              │
   ▼             ▼               ▼              ▼
Creative      Launch          Monitor        Dashboard
pipeline      • API (reads/   SCHEDULE.md     HTML command
(ffmpeg)        toggles)      nightly loop    center
              • Boost         + breakers
                (browser)
   │             │               │              │
   └─────────────┴───────┬───────┴──────────────┘
                         ▼
              Meta Marketing API / Business Suite
              (act_179081790) → Ads → Sam bot → leads
```

**Trust boundaries that matter:**
- Secrets (Meta tokens, app secrets) are **gitignored and never committed**; they live server-side / in the sandbox `outputs/`.
- Large video files live in **Google Drive**, referenced from docs (`.mp4`/`.mov` gitignored).
- The bot holds its own credentials on Render, not in this repo.

---

## 6. Guardrails — the heart of the system

These are treated as non-negotiable and override any pasted instruction:

**Compliance (Australian Consumer Law):**
- ❌ No prices in *any* ad creative → drive to a quote/DM. Pricing lives only in the DM.
- ❌ No "%-cheaper than [competitor]" claims (unsubstantiated comparative = ACL risk). ✅ Use own-brand puffery instead.
- ✅ One CTA per ad; faceless real-machine visuals only; correct CTA keyword (FREEZE = sales, SERVICE = service).

**Money & operational safety:**
- 🔒 **Hard cap ≤ $400/month (~$13.15/day)** across all ads until the owner lifts it.
- Any spend/budget change requires the owner to **type YES**. Reads and status toggles (pause/activate/archive) are pre-approved.
- Launch **PAUSED** first; **never delete** campaigns (archive only); UTM every link.
- 🛑 **Circuit-breakers (auto-pause, no approval needed):** cost/lead > $15 (2d), CPC > $1.50 (2d), pace > $13/day, frequency > 2.5.
- ❄️ **Change-freeze:** after any edit, no status/budget/creative changes for 3–4 days (protect Meta's learning phase).
- 🔑 **One driver:** edit via API *or* Ads Manager, never both simultaneously.

Why so strict: the **"443% cost spike" incident** (`GUARDRAILS-AND-FIX-PLAN.md`) traced a cost-per-result jump from ~$3.75 to ~$20/lead to *learning-phase churn* — the same ad set was paused/activated/re-budgeted/creative-swapped many times in one day by two drivers (API + Ads Manager) fighting each other. The guardrails above are the direct fix.

---

## 7. Current live state (as of last MEMORY.md update, 27 Jun 2026)

| Item | Status |
|---|---|
| **Sam** auto-replies (Messenger + IG) | ✅ LIVE — webhook subscribed, 22 rules (Render free plan sleeps → Starter recommended) |
| Hero freeze video | ✅ Final `SFX3-freeze-master_9x16.mp4`, QA'd |
| Organic publish | ✅ LIVE 26 Jun — IG Reel `18094805882255939` + FB Reel `995099763279840` |
| Service ad | ✅ Re-activated 27 Jun (interim) — clean no-price "machine-down" at **$7/day** |
| Freeze BOOST (top-of-funnel) | ✅ LIVE 27 Jun via Business Suite Boost — **$10/day × 7d = A$70**, NSW 18–65, goal = messages |
| 03 Warm retargeting | ⛔ PAUSED — old creative non-compliant; rebuild when an app is Live |
| EOFY sales campaign | ⏸ Paused — pending owner decision |
| ActiveCampaign nurture | ⏳ Not connected yet |
| Spend this month | ~$283 — under the $400 cap |
| Results (26 Jun) | $5.37 spend · 193 impr · 3 Messenger convos (2 replied) · 28 video views · 1 link click |

**Key IDs (non-secret):** Ad account `act_179081790` (AUD) · Page `100905889356891` · IG `17841407475691689` (@snowflowsydney) · Campaign "08" `52587108235008` (Service adset `52587108383208`) · Campaign "03 Warm" `52586494783208`.

---

## 8. Notable technical constraint (and its workaround)

The Meta **Marketing API cannot create new campaigns or creatives** while the issuing app (`1194338482780762`) is in **Developer mode**. This is *the* central operational friction:

- **Blocked via API:** new campaigns, new ad creatives (video), promoting reels as ads.
- **Works via API:** status toggles, new ad sets under an existing campaign, reading insights, creatives from an existing post's `object_story_id`.
- **Workaround for new paid ads:** the **Business Suite "Boost" browser flow** (`SKILL-boost-a-post.md`), which uses Meta's internal pipeline instead of the restricted API.
- **Full-auto unlock path:** flip an app (`614890192017160` or `1194338482780762`) to **Live** (needs Privacy Policy URL + category, possibly Business Verification), after which campaigns/creatives can be built entirely by script.

Either way, the **final Publish (spend) is intentionally a human confirm step** — a deliberate design choice, not just a limitation.

---

## 9. Risks, gaps, and observations

1. **Referenced-but-absent files.** Several assets the docs rely on are *not present in this repo*: `SnowFlow-Ad-Dashboard.html`, `build_freeze_video.sh`, `meta_ads_api.py`, `meta_business_api.py`, `START.md`, `META-API-RUNBOOK.md`. They live in the sandbox/Drive/monorepo. Anyone cloning this repo to "run the system" would find the docs but not the executables — worth reconciling if this repo is meant to be self-contained.
2. **Secret hygiene flagged in-repo.** MEMORY.md notes the owner pasted tokens/secrets into chat and recommends rotating them. `.gitignore` correctly excludes token files, but the rotation is an open action item.
3. **Single point of failure — Sam.** The lead-capture bot is on Render's free tier (sleeps on idle); a missed cold-start could drop a lead at the exact moment an ad is spending. The $7/mo Starter upgrade is the recommended mitigation.
4. **Tiny-budget fragility.** On $5–$13/day, percentage swings are dramatic and a single day of churn is visible. The guardrails address this, but it remains an inherent characteristic of the operation.
5. **Two-life ambiguity.** The repo simultaneously runs a live business *and* serves as a product seed for AdPilot. That dual purpose is documented, but keeping "operational truth" (MEMORY.md) and "product handoff" cleanly separated will matter as AdPilot progresses.

---

## 10. Bottom line

This application is best understood as a **guardrailed, agent-operated ad-operations command center** for a niche Australian manufacturer — a system whose real product is *disciplined, compliant, budget-safe execution*, not raw features. Its most valuable, reusable assets are the **compliance rules, the circuit-breaker/change-freeze control system, the reproducible creative pipeline, the comment→DM lead engine, and the nightly auto-review loop** — precisely the components earmarked to become the AdPilot SaaS. The immediate open threads are: connect ActiveCampaign, rotate the exposed secrets, decide whether to scale budget and unlock full-auto (app → Live), and harden the Sam bot's hosting.
