# MASTER_CONTEXT.md — Clauk Project

> **Format:** Portable Markdown archive. Self-contained.
> **Purpose:** Reconstruct the full Clauk project context for Git storage and ingestion by any LLM (Claude, ChatGPT, Gemini, Cursor, Copilot, Windsurf, local agents).
> **Generated:** 21 May 2026
> **Source material:** Original project PRD (2 May 2026), Universal UI/UX Skill Pack v1.0, 26+ rounds of strategy conversation (8–12 May 2026), prototype HTML, PRD v1/v2/v3 iterations.
> **Confidence markers:** Statements marked `[evidence]` come from preserved artifacts; `[inferred]` from conversation reconstruction; `[uncertain]` when sources conflict or are incomplete.

---

# Project Overview

**Name:** Clauk
**Domain (planned):** clauk.app (not yet registered as of 21 May 2026)
**License:** MIT (open-source from Day 7 of build week)
**Stage:** Pre-launch. PRD v3 locked. V1 build week scheduled to begin "Monday" after PRD approval. Prototype HTML exists at `clauk-prototype.html` (~137 KB, 2,229 lines) as a visual/functional reference.

**Product type:** Privacy-first, mobile-first, local-first AI memory layer delivered as a PWA (Progressive Web App). Cross-LLM context bridge.

**Tagline:** *"Free AI memory + free AI for students. Built on your phone."*

**Core idea (locked as D-3001):** *"The structured memory layer for people who'd rather not have an AI watching them."*

**Slogan:** *"Save your AI context once. Carry it to any AI. Without the surveillance tax."*

## Purpose
Solve the pain of context loss when users switch between AI tools (Claude, ChatGPT, Gemini, Perplexity, Grok, Le Chat, Mistral). Today users hand-roll prompt templates, build keyboard macros, or manually copy-paste between apps. Clauk gives them a structured local vault + multi-format export so the same context can be reused across any AI tool without re-explaining themselves.

## Goals
1. Capture, structure, and store AI conversation context locally with hardware-backed encryption.
2. Export to universal formats (`cm.md`, `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`, `.cursor/rules/*.mdc`, JSON).
3. Provide bundled free LLM access (Gemini + Groq + OpenRouter passthrough) to students who cannot afford $720/yr for three paid AI subscriptions.
4. Stay independent (no VC, no data monetization, no surveillance) and reach indie-sustainable revenue (~$30 K Year 1, ~$30 K target).
5. Build trust through portability (migrate-friendly) rather than lock-in.

## Users (multi-region, locked V3)

| Persona | Region | Profile | Willing to pay |
|---|---|---|---|
| **Aryan** — Computer Science student | Bangalore, India | 21, BTech 3rd year, mid-range Android, ~₹500–2,000/mo discretionary | **₹2,000/year ($24)** |
| **Maya** — PhD researcher | Manchester, UK | 26, social sciences, iPhone, £10–30/mo subscription budget | **£19/year ($24)** |
| **Marco** — Indie hacker | Austin, USA | 32, building 3rd SaaS attempt, already pays $60/mo across AI tools | **$29 lifetime / $24/year** |

## Business intent
Lifestyle / independent business path (not VC). Investor verdict logged: *"Promising founder, real wedge, but $0-forever consumer mobile = lifestyle math. Track for 12 months."* Founder explicitly chose Path A (independent) over Path B (make it investable). Target $30 K ARR by Month 12; sustain on solo founder + $5/mo Cloudflare burn.

---

# Current State

## What exists right now `[evidence]`

| Artifact | Path | Status |
|---|---|---|
| Initial PRD | `/mnt/project/C_PRD_2_May` | Historical — describes earlier "Smart Harvest" vision (Android Quick Settings + accessibility-tree parsing + OCR). Superseded but contains foundational problem framing. |
| Universal UI/UX Skill Pack v1.0 | `/mnt/project/ui-skill` | Locked design system. Tokens, components, interaction patterns, 7-check protocol, 10-priority framework. To be applied unchanged. |
| Visual mockup | `/mnt/user-data/outputs/clauk-mockup.html` | ~40 KB · 8 May · early single-screen visual concept |
| Canonical prototype | `/mnt/user-data/outputs/clauk-prototype.html` | ~137 KB · 2,229 lines · 12 May · Working V3 prototype with Day-6 quick wins integrated. **State note:** hero/onboard partially pivoted to student-first framing during 12 May session; some elements remain in pre-pivot state. Inconsistent intermediate state. |
| PRD v1 | `/mnt/user-data/outputs/clauk-week1-prd.md` | ~18 KB · 12 May 12:51 |
| PRD v2 | `/mnt/user-data/outputs/clauk-week1-prd-v2.md` | ~28 KB · 12 May 16:41 · Pricing pivots locked |
| PRD v3 (canonical) | `/mnt/user-data/outputs/clauk-prd-v3.md` | ~28 KB · 12 May 16:54 · **BINDING SCOPE DOC** — student-first, multi-region, MIT-only |

## What works `[evidence from prototype]`
- IndexedDB schema with encrypted card storage
- Capture flow with paste from clipboard
- Card list with search and filter
- Multi-format export (6 formats)
- Privacy Verifier panel (live fetch interceptor, storage status, share-target registration)
- Settings page with i18n stub (en/hi/es/pt-BR locales)
- Web Share Target API integration (`?share=1` handler)
- iOS install overlay (detect iPhone/iPad UA + standalone state)

## What is incomplete `[inferred from PRD-vs-prototype delta]`
- Free LLM proxy (Cloudflare Worker) — not deployed; UI is placeholder
- "Ask Clauk's free AIs" launchpad section — UI exists, backend stub only
- AI Access Settings section — partially mocked, not wired to actual quota
- Card of the Day surfacing — designed in PRD v3 §6.5, not in prototype
- Open Threads filter chip — designed, not in prototype
- Streak counter — designed for V1.1
- Continue conversation mode — designed, not implemented
- Smart auto-categorization on paste — designed (regex/heuristics), not implemented
- Multi-currency display (₹/£/$) — designed in PRD v3 QW10, not in prototype
- `/privacy` page with GDPR/DPDPA/CCPA sections — designed in QW11, not yet created
- `/in` India landing page with Hindi tagline + Razorpay — designed in QW12, not yet created
- Stripe Payment Link + Razorpay Payment Page — designed for V1.2 Week 4, not started
- Hero rewrite to student-first framing — partially applied in prototype, incomplete

## What is broken / risky `[inferred]`
- **Prototype state inconsistent.** Hero/onboard was partially updated to student-first; Card of the Day insertion was started but truncated mid-edit on 12 May. Visual state mixes pre-pivot and post-pivot copy. Decide rebuild-from-PRD-v3 vs continue-from-prototype before Day 1.
- **No domain registered.** clauk.app/clauk.dev/clauk.io all unbought as of 21 May.
- **No GitHub org created.**
- **Razorpay KYC not started.** This is the longest dependency (3–7 days in India).
- **No production Cloudflare Worker.** Free LLM proxy is currently stubbed alerts.
- **No production demo video.** Required for Day 7 publish + Week 2 launch.

---

# Architecture

## High-level diagram (mental model)

```
┌──────────────────────────────────────────────────────────┐
│  Clauk PWA (clauk.app · Cloudflare Pages · static)       │
│  ┌────────────────────────────────────────────────────┐  │
│  │ HTML/CSS/Vanilla JS (no framework V1)              │  │
│  │ ├── Service Worker (PWA shell + Web Share Target)  │  │
│  │ ├── IndexedDB via idb (AES-GCM 256 encrypted)      │  │
│  │ ├── localStorage backup (last 50 cards on iOS)     │  │
│  │ └── i18n: t() function + en/hi/es/pt-BR JSON       │  │
│  └────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────┘
                          │
                          │  (only on explicit "Ask AI" tap)
                          ▼
┌──────────────────────────────────────────────────────────┐
│  Cloudflare Worker proxy (proxy.clauk.app, $5/mo)        │
│  ┌────────────────────────────────────────────────────┐  │
│  │ Rate-limit per-fingerprint (50/day Gemini)         │  │
│  │ Rate-limit per-IP (100/hr at edge)                 │  │
│  │ Zero user logs                                     │  │
│  │ Multi-provider fallback chain                      │  │
│  └────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────┘
                          │
                          ▼
┌──────────────────────────────────────────────────────────┐
│  Free LLM APIs (downstream, all free tier)               │
│  ├── Gemini AI Studio       1,500 req/day                │
│  ├── Groq Llama 3.3         14,400 req/day               │
│  └── OpenRouter :free       rate-limited models          │
└──────────────────────────────────────────────────────────┘

╞════ V1.2 additions (not Day 1) ════╡

┌──────────────────────────────────────────────────────────┐
│  Stripe Payment Links / Razorpay Payment Pages           │
│  (hosted checkout — Clauk redirects only, no backend)    │
└──────────────────────────────────────────────────────────┘
```

## Storage layout `[evidence from prototype + PRD v3]`

| Store | Key | Value |
|---|---|---|
| IndexedDB `clauk-vault.cards` | UUID v4 | Encrypted card object: `{id, type, title, content, tags[], scope, source, status?, confidence?, ts_created, ts_updated, ts_last_used, use_count}` |
| IndexedDB `clauk-vault.meta` | `streak`, `lastCaptureDate`, `cardOfDayId`, `quotaUsed` | Counter values |
| localStorage `clauk_settings` | — | Locale, theme, BYOK keys (encrypted), display prefs |
| localStorage `clauk_sync_waitlist` | — | Array of `{email, ts}` for V1.2 launch comms |
| localStorage `clauk_last_50` | — | iOS-only backup of 50 most recent cards (D-2902 dual-write) |

## Card types (10, locked) `[evidence from PRD v2]`
Bio · Role · Project · Goal · Decision · Discovery · Preference · Style · Person · Snippet

Two card types carry extra fields:
- **Decision** has `status: pending | resolved | abandoned`
- **Discovery** has `confidence: hypothesis | validated | invalidated`

These two fields drive the "Open Threads" filter chip on home (D-3302 layer L2).

## Export format priority `[evidence from D-2003]`
1. `cm.md` — Clauk-native Markdown format with YAML frontmatter
2. `AGENTS.md` — Linux Foundation universal standard for AI agents
3. `CLAUDE.md` — Anthropic Claude Code convention
4. `GEMINI.md` — Google Gemini convention
5. `.cursor/rules/*.mdc` — Cursor IDE convention
6. JSON — raw machine-readable export

All exports are **one-way**. No re-import in V1 (ZIP import is V1.2 Month 2). The export-then-leave path is intentional: it's the migrate-friendly trust moat.

## Capture surfaces (6, sequenced) `[evidence from D-2207]`

| # | Surface | Ships in |
|---|---|---|
| 1 | Paste from clipboard | V1 Day 4 |
| 2 | Web Share Target (Android system share sheet) | V1 Day 6 (QW2) |
| 3 | Manual type/edit | V1 Day 4 |
| 4 | Starter pack install (10 examples) | V1 Day 3 |
| 5 | Browser extension (Manifest V3, activeTab) | V1.2 Month 2 |
| 6 | ZIP import (ChatGPT/Claude/Gemini export bundles) | V1.2 Month 2 |

## Permanently excluded capture surfaces `[evidence from D-2606, D-2905]`
- Screen capture / passive surveillance (Rewind/Mirix territory)
- Browser-agent automation (Comet's lane)
- Login bridge / session hijacking (browser security violation + ToS violation)
- Accessibility-tree parsing for live AI apps (originally in May 2 PRD — abandoned, see Chronological Timeline)

---

# Repository and File Structure

**Planned Git repo layout** (target — does not yet exist physically) `[inferred from PRD v3 §7-8]`:

```
clauk-app/clauk/                    (GitHub, MIT license, public Day 7)
├── README.md                        (links to live PWA + 1-line product description)
├── LICENSE                          (MIT)
├── MASTER_CONTEXT.md                (this file)
├── clauk-prd-v3.md                  (binding scope doc, locked 12 May)
├── public/
│   ├── index.html                   (the single-file PWA — current prototype basis)
│   ├── manifest.json                (PWA manifest + Web Share Target declaration)
│   ├── sw.js                        (Service Worker — share target + offline)
│   ├── icons/                       (PWA icons: 192, 512, maskable)
│   ├── locales/
│   │   ├── en.json                  (complete)
│   │   ├── hi.json                  (stub V1, full V1.1)
│   │   ├── es.json                  (stub V1)
│   │   └── pt-BR.json               (stub V1)
│   ├── privacy.html                 (GDPR + DPDPA + CCPA sections — QW11)
│   ├── in/index.html                (India landing — Hindi + INR — QW12)
│   ├── vs/
│   │   ├── mempalace.html           (comparison page — Week 2+)
│   │   ├── supermemory.html
│   │   ├── ai-context-flow.html
│   │   └── mem0.html
│   └── lib/
│       ├── idb.js                   (Jake Archibald MIT, ~8 KB)
│       ├── fuse.min.js              (Apache 2.0, ~14 KB, used when card count > 100)
│       └── marked.min.js            (MIT, Markdown preview rendering)
└── worker/
    ├── llm-proxy.js                 (Cloudflare Worker — Gemini + Groq + OpenRouter)
    ├── wrangler.toml                (Cloudflare deployment config)
    └── README.md                    (proxy setup instructions)
```

## Current physical artifact map `[evidence]`

| Path | Purpose |
|---|---|
| `/mnt/project/C_PRD_2_May` | Original problem statement and "Smart Harvest" feature spec (2 May 2026) |
| `/mnt/project/ui-skill` | Universal UI/UX Skill Pack v1.0 — design system |
| `/mnt/user-data/outputs/clauk-mockup.html` | Early visual concept (8 May) |
| `/mnt/user-data/outputs/clauk-prototype.html` | Canonical V3 prototype (12 May) — to be lifted into `public/index.html` |
| `/mnt/user-data/outputs/clauk-week1-prd.md` | Historical PRD v1 |
| `/mnt/user-data/outputs/clauk-week1-prd-v2.md` | Historical PRD v2 |
| `/mnt/user-data/outputs/clauk-prd-v3.md` | **Binding canonical PRD** |

---

# Core Components

## 1. Capture loop
- **Trigger:** paste, share intent, manual type, starter pack install
- **Processing:** smart auto-categorization (regex/heuristics, no AI inference cost) infers: card type, source LLM, suggested title, suggested tags, confidence chip
- **Output:** structured draft for user review; on save, encrypted to IndexedDB
- **Critical principle:** raw extracted text and user-edited version both preserved (D-201 from May 2 PRD: never silently mutate user input)

## 2. Vault (IndexedDB + encryption)
- AES-GCM 256 via Web Crypto API
- Key derived per-device, never leaves the device
- `idb` wrapper from Jake Archibald (MIT, ~8 KB) — industry standard
- iOS fallback: localStorage backup of last 50 cards (defends against IndexedDB eviction)
- `navigator.storage.persist()` requested on first save (QW1)

## 3. Smart Launchpad
- Verified deep-link table for Claude, ChatGPT, Perplexity (auto-prefill works)
- Open-and-paste-guide for Gemini, Grok, Le Chat, Mistral (no deep-link support)
- **Two modes per card:** "New question" (default) vs "Continue conversation" (wraps as continuation prompt — solves the Sand-West F5-macro pain found on Reddit)
- "Ask Clauk's free AIs" subsection — buttons for Gemini (50/day) + Groq (500/day) using Cloudflare Worker proxy

## 4. Privacy Verifier
- Live fetch/XHR interceptor count (proves "0 network requests" claim)
- Persistent storage status
- Share Target registered status
- Encryption-at-rest status
- Last export timestamp
- Cookie-free verified (always zero)

## 5. Daily-return monopoly (3 layers, D-3302)

| Layer | Mechanism | Effort |
|---|---|---|
| **L1 Mechanical** | Daily Gemini/Groq quota reset = user must open Clauk to use today's free quota | Already in V1 |
| **L2 Re-engagement** | Card of the Day + Open Threads filter chip on home | V1 Day 5 (~55 min) |
| **L3 Habit** | Streak counter + daily reflection prompt | V1.1 (~2 hrs) |

## 6. Monetization rail (V1.2, deferred)
- Stripe Payment Links (UK, USA, EU, Global)
- Razorpay Payment Pages (India — UPI + cards)
- Lemon Squeezy fallback for EU (auto-handles VAT)
- No backend in V1: Payment Links generated in dashboards, Clauk only redirects
- License verification deferred to V1.2 (open question: signed JWT vs lookup endpoint)

## 7. Compliance posture (zero-data architecture)
- No cookies (only localStorage + IndexedDB)
- No telemetry / analytics
- No personal data leaves device for core features
- Free LLM passthrough is the only outbound traffic; explicit per-call disclosure required (D-3412)
- `/privacy` page with GDPR + DPDPA + CCPA sections (QW11)

---

# Chronological Timeline

| Date | Phase | Event |
|---|---|---|
| **2 May 2026** | Initial PRD | "Smart Harvest" vision: privacy-first context bridge with Android Quick Settings tile, accessibility-tree parsing, screen-aware OCR fallback. Defined IDIS framework, 6 core features (F1-F6), user flows, AI/ML risk analysis. **Primary capture surface: accessibility tree.** |
| **8 May 2026** | Design system | Universal UI/UX Skill Pack v1.0 created. Locks tokens, 10-priority framework, 7-check protocol, mobile-first 375px, MIT/Apache deps only. Identifies the product as Tool × Productivity × Privacy-first × Mobile-first archetype. |
| **8 May 2026** | First visual | `clauk-mockup.html` (~40 KB) — early single-screen concept |
| **8–12 May 2026** | Strategy rounds 1–14 | Pivoted from accessibility-tree capture (Android-only, fragile) to paste + Web Share Target capture (works everywhere). Locked 10 card types. Locked 6 capture surfaces (sequenced). Locked 6 export formats. Mapped 13+ competitors. Investigated MCP integration (rejected for V1 — too dev-only). Locked PWA over React Native (D-2602). Locked `idb` library. |
| **12 May 2026 morning** | PRD v1 | `clauk-week1-prd.md` (~18 KB) generated |
| **12 May 2026 afternoon, rounds 15–22** | Multi-region pricing pivot | Investigated MemPalace, Plurality AI Context Flow, myNeutron, Supermemory, Mem0, brain-mcp, MemVid, Comet, Rewind/Limitless, Screenpipe. Locked steal-and-refuse master list (D-3107). Decided NOT to compete with Perplexity Comet's browser-agent lane or Rewind/Limitless's passive-capture lane. **Refusal IS the moat** insight crystallized. |
| **12 May 2026 round 23** | PWA limits research | Researched iOS PWA constraints (7-day eviction, 50 MB cache, fragile IndexedDB). Locked D-2901 (iOS install overlay), D-2902 (dual-write localStorage backup), D-2903 ("open weekly" toast). |
| **12 May 2026 round 24** | Login-bridge research | Investigated session-bridge / cookie-injection automation for opening user's logged-in AI accounts. **Permanently rejected** (D-2905) due to browser security + ToS violation + user-hostile architecture. Locked 5 legitimate bridges instead (D-2906). |
| **12 May 2026 round 25** | PRD v2 | `clauk-week1-prd-v2.md` (~28 KB) — 40 enforced requirements, 31 acceptance criteria, decisions D-2001 through D-3304 |
| **12 May 2026 round 26** | Core idea reframe | Locked D-3001: *"The structured memory layer for people who'd rather not have an AI watching them."* Locked D-3002: do not compete with Comet or Rewind lanes. Locked D-3003: smart auto-categorization on paste as quick win. |
| **12 May 2026 round 27** | Investor verdict | Honest investor read delivered. Score: ~5.7/10 weighted average. Verdict: "interesting, not investable on standard VC terms." Three internal scenarios modeled (bullish 25%, neutral 60%, bearish 15%). Two paths surfaced: Path A (independent / lifestyle) vs Path B (make it investable). |
| **12 May 2026 round 28** | Revenue target locked | Founder set explicit target: **$5–10 K USD revenue in 3–9 or 12 months**. This locked Path A. Drove rethink of pricing timing: charge from Week 1, not V3 Month 4. |
| **12 May 2026 round 29** | Student-first pivot | Founder explicitly pivoted to *"students + cannot-afford-builders"* as primary audience. Deferred all monetization decisions, focused on user pain. Free Gemini passthrough promoted from V2 to V1 Day 6 (D-3202). |
| **12 May 2026 round 30** | Lunch-money pricing + multi-region | Vision crystallized: *"one lunch = one year"*. Multi-region expansion: India primary (₹2,000/yr), UK secondary (£19/yr), USA tertiary ($24/yr). Founder's Edition $29 lifetime, 500-spot cap. **D-3401 through D-3422 locked.** |
| **12 May 2026 round 31** | PRD v3 generated | `clauk-prd-v3.md` (~28 KB, 19 sections) — **binding scope doc** |
| **12 May 2026 round 31 end** | Day closed | Founder said: "No need to create HTML now... save memory local." Session compacted to free context. |
| **21 May 2026** | This document | MASTER_CONTEXT.md generated for Git portability + future LLM ingestion |

## Key pivots in order

1. **Capture mechanism pivot** (rounds 1–5): accessibility-tree parsing → paste + Web Share Target
2. **Platform pivot** (rounds 6–10): React Native cross-platform → PWA single codebase
3. **MCP decision** (rounds 10–14): considered → rejected for V1 (V2.2 reconsider)
4. **Audience pivot 1** (rounds 15–20): general AI power users → free-tier mobile users
5. **Pricing pivot 1** (rounds 21–25): revenue at V3 Month 4 → revenue at Week 1
6. **Audience pivot 2** (rounds 28–29): free-tier mobile users → students + cannot-afford-builders
7. **Regional expansion** (round 30): single-currency → multi-region (INR/£/$/€)
8. **Strategic positioning lock** (round 30): refusal IS the moat (vs trying to out-feature Comet)

---

# Key Decisions and Rationale

This project carries ~102 locked decisions across three PRD versions. Below is the curated subset most critical for handoff. Full decision log lives in `clauk-prd-v3.md` §5 and `clauk-week1-prd-v2.md`.

## Foundational principles (D-101 through D-401)

| # | Decision | Rationale |
|---|---|---|
| D-201 | Never silently mutate user input. Preserve raw + cleaned + final versions. | Trust through auditability. AI extraction is probabilistic; raw must remain available. |
| D-401 | Three promises: (1) no paywall on core features ever, (2) stays on device, (3) works in any AI | Marketing trust anchor; constrains all future feature decisions |

## Platform and stack (D-1101, D-2601, D-2602, D-3414-3420)

| # | Decision | Rationale |
|---|---|---|
| D-1101 | i18n scaffold from Day 1 (en/hi/es/pt-BR) | Adding it later requires UI rewrite; cheap up front |
| D-2601 | Use `idb` for IndexedDB | Industry standard, MIT, 8 KB; avoids hand-rolled Promise wrappers |
| D-2602 | PWA over React Native or Tauri for V1 | Single codebase, one deploy, no app-store cut, instant updates |
| D-3414 | Stack 100% MIT or Apache 2.0 — no AGPL, no GPL, no proprietary | Open-source from Day 7; forkable; trust signal |
| D-3415 | No build system in V1 | Single HTML file, inline JS/CSS; complexity earned later if needed |
| D-3416 | No framework in V1 | Vanilla JS — saves ~150 KB bundle; revisit V2.0 |
| D-3417 | No backend except free LLM proxy | Frontend-first scope; $5/mo burn cap |
| D-3418 | Free LLM proxy = Cloudflare Worker with multi-provider chain | Gemini + Groq + OpenRouter free tiers stack to ~1,000+ queries/day per user |
| D-3419 | Open-source from Day 7 (GitHub MIT) | Forkability is a trust signal; aids SEO via comparison pages |
| D-3420 | Self-hostable as a feature | "Don't trust us? Run it yourself" — strongest possible privacy claim |

## Capture and storage (D-2003, D-2207, D-2901-2903)

| # | Decision | Rationale |
|---|---|---|
| D-2003 | Export defaults: `cm.md` + `AGENTS.md` | One is Clauk-native, the other is universal standard; covers both lock-out and forward-compat |
| D-2207 | 6 capture surfaces sequenced over 12 months | Don't try to do all on Day 1; ship V1 with #1-4 |
| D-2901 | iOS install overlay (detect UA + standalone) | iOS Safari evicts non-installed PWA storage after 7 days; install is existential |
| D-2902 | Dual-write last 50 cards to localStorage on iOS | Defensive layer against IndexedDB eviction |
| D-2903 | "Open weekly" educational toast on iOS first save | Soft education on persistence behavior |

## Refusals (permanent, D-2606, D-2905, D-3002)

| # | Decision | Rationale |
|---|---|---|
| D-2606 | No screen capture / passive surveillance ever | Mirix / Rewind territory; destroys trust moat |
| D-2905 | No login bridge / session hijacking / cookie injection | Browser security + ToS + user-hostile architecture |
| D-3002 | Do not compete with Comet (browser agent) or Rewind/Limitless (passive capture) | $21 B + 700-engineer competitor (Comet); cautionary tale (Rewind→Meta). Refusing those lanes IS the moat. |

## Five legitimate bridges (D-2906) — replaces the rejected login-bridge concept

1. Deep links (V1) — claude.ai/chats, chat.openai.com, gemini.google.com prefill where supported
2. Web Share Target (V1 Day 6) — receive shares from any app
3. Browser extension (V1.2) — Manifest V3 with `activeTab` only
4. BYOK API (V2) — user provides their own keys; stay client-side
5. Free LLM passthrough (V1 Day 6+ backend) — Cloudflare Worker proxy with rate limits

## Audience and pricing (D-3201-3206, D-3301-3304, D-3401-3405)

| # | Decision | Rationale |
|---|---|---|
| D-3201 | Primary audience is students + builders who can't afford 3 paid AIs | $720/yr for ChatGPT + Claude + Gemini is unaffordable to target audience; concrete pain |
| D-3202 | Free Gemini passthrough ships V1 Day 6 (promoted from V2) | This is the student wedge; the memory layer becomes a gift inside the AI-access wrapper |
| D-3301 | Lunch-money pricing: ₹2,000 / £19 / $24 annual, ₹2,500 / £23 / $29 lifetime (500 cap) | Impulse-purchase price band; affordable across all target regions |
| D-3302 | Three-layer daily-return monopoly (mechanical/re-engagement/habit) | No single mechanism is enough; layered defense against churn |
| D-3303 | Monopoly is Audience + Trust + Bundle, not tech | Can't outspend Mem0, can't outbuild Comet; can own a tribe |
| D-3304 | Migrate-friendly = trust moat | Every "leaving Clauk" moment becomes proof we respected the user |
| D-3401 | India primary market (INR pricing + Razorpay) | Largest volume opportunity at the lunch-money price point |
| D-3402 | UK + USA secondary (GBP/USD via Stripe) | Lower volume, higher ARPU; balances mix |

## Compliance posture (D-3406-3413)

| # | Decision | Rationale |
|---|---|---|
| D-3406 | Local-first means most compliance is automatic | Zero data collected = most GDPR/DPDPA/CCPA obligations moot |
| D-3410 | Cookie-free architecture | No consent banner required in EU/UK |
| D-3411 | Payment data handled exclusively by Stripe/Razorpay | Clauk never sees card numbers; compliance burden ~zero |
| D-3412 | Free LLM passthrough requires per-call disclosure | "Your question goes to Google AI Studio; their policy applies" |
| D-3413 | No telemetry/analytics in V1 | Opt-in only V1.2+, never default-on |

## Steal-and-refuse master list (D-3107)

**STEAL:**
- MemPalace comparison-page playbook (`/vs/X` pages)
- brain-mcp cognitive-state UX (status, confidence fields)
- MemVid "single portable file" framing
- Plurality AI Context Flow buckets UI
- myNeutron lifetime-deal financing
- Supermemory startup-credit program (Year 2)
- Mem0 industry-report citations (in launch posts)

**REFUSE:**
- Per-token billing (Supermemory)
- Cloud-first architecture (Plurality, Mem0)
- Screen capture (Rewind, Mirix)
- Browser-agent lane (Comet)
- VC funding pressure dynamic (Rewind→Meta cautionary tale)
- Medical metaphors (regulatory + brand collision)
- App Store distribution V1 (30% cut + review delays)
- Account requirement (local-first removes need)

---

# Constraints and Non-Negotiables

## Technical constraints (locked, do not change without explicit founder approval)
- **MIT or Apache 2.0 dependencies only.** No AGPL, no GPL, no proprietary. Verify license on every new dep.
- **No framework in V1.** Vanilla HTML/CSS/JS. Single file. Inline assets.
- **No backend except the Cloudflare Worker LLM proxy.** Everything else client-side.
- **No cookies.** Use localStorage + IndexedDB only.
- **No telemetry.** No analytics. No tracking. No fingerprinting beyond rate-limit hash (one-way SubtleCrypto digest of `userAgent + screen + timezone`).
- **No account system.** Anonymous-first.
- **No data sale, ever.** This was in the original May 2 PRD and was explicitly removed in PRD v2 — anything resembling data monetization is permanently forbidden.
- **Hardware-backed encryption.** AES-GCM 256 via Web Crypto. Keys never leave device.
- **iOS PWA install required for retention.** D-2901-2903 stack is non-negotiable on iPhone/iPad.

## Business constraints
- **Free forever for core features.** Capture, export, vault, ≥50 free Gemini/day, ≥500 free Groq/day all stay free. Permanent.
- **Solo founder pace.** All scope decisions assume one builder, 40 hrs/week max.
- **$5/mo infrastructure burn cap (V1).** If burn exceeds this, rate-limit more aggressively or pause new signups.
- **No VC funding accepted.** Path A (independent) is locked.
- **Founder's Edition capped at 500 forever.** When sold out, lifetime tier closes. Honesty + scarcity.

## Forbidden approaches (will be rejected by the project on sight)
- Screen capture, screen recording, OCR-driven passive capture
- Browser-agent task automation
- Login bridge, cookie injection, session hijacking
- Native mobile apps in V1 (PWA only)
- Subscriptions on core features
- Ads of any form
- User-data sale or licensing
- Closed-source / proprietary deps

## "Do not change" areas
- The three promises (D-401) — these are public commitments
- Card type list (10 types) — schema migration risk if changed
- Export format list (6 formats) — downstream integrations depend on these
- Cookie-free architecture — flipping this triggers EU/UK banner requirement
- Anonymous-first / no-account — feature gate that defines the audience

---

# Current Priorities

## Highest priority (blocking V1 launch)

| # | Task | Why it blocks |
|---|---|---|
| 1 | Founder writes "PRD v3 locked" approval | Closes scope debate; build starts Day 1 |
| 2 | Domain registration (clauk.app / .dev / .io) | Day 7 publish requires DNS |
| 3 | GitHub org creation (`clauk-app` vs alternatives) | Day 7 publish requires repo |
| 4 | Razorpay KYC initiated | 3–7 day delay is longest dependency for India launch |
| 5 | Decide: continue editing existing prototype vs rebuild from PRD v3 | Architectural fork |
| 6 | Generate Day 1 Claude Code build prompt | Feeds Monday morning execution |

## Active workstreams (V1 build week)
- **Day 1 Mon:** Scaffold · `idb` setup · IndexedDB schema · AES-GCM · file structure
- **Day 2 Tue:** Capture flow · paste · auto-categorization heuristics · save to vault
- **Day 3 Wed:** Card list · search · Card of the Day · Open Threads chip · streak scaffold
- **Day 4 Thu:** Launchpad · deep-link table · Continue conversation mode · Ask Clauk's free AIs UI · BYOK key entry
- **Day 5 Fri:** Settings · Privacy Verifier · AI Access · multi-currency · `/privacy` page
- **Day 6 Sat:** All 12 quick wins · iOS install overlay · Web Share Target · multi-format export · `/in` landing
- **Day 7 Sun:** QA against A1-A40 · Lighthouse 95+ · Cloudflare Pages deploy · GitHub public · demo video

## Adjacent workstreams
- Hindi translation for `/in` (DIY vs hire — open)
- Cloudflare Worker free LLM proxy (founder deploys vs Claude Code builds — open)
- Demo video format (60s vertical vs 90s horizontal — open)

---

# Open TODOs

## Explicit `[evidence from PRD v3 §17]`
- [ ] Approve PRD v3 in writing
- [ ] Register domain
- [ ] Create GitHub org
- [ ] Start Razorpay KYC
- [ ] Generate Day 1 Claude Code build prompt
- [ ] Decide rebuild-from-prototype vs rebuild-from-PRD-v3
- [ ] Hindi translation for `/in` page
- [ ] License verification design for V1.2 (JWT vs lookup)
- [ ] Founder's Edition cap decision (500 vs 1,000)
- [ ] Demo video format choice

## Implicit (inferred from prototype state and PRD v3)
- [ ] Finish hero rewrite in prototype to fully match student-first framing
- [ ] Insert Card of the Day component in prototype home view
- [ ] Insert Open Threads filter chip in prototype list view
- [ ] Add AI Access section to Settings (currently a partial mockup)
- [ ] Add multi-currency display logic (`navigator.language` detection)
- [ ] Build `/privacy` page with GDPR + DPDPA + CCPA sections (QW11)
- [ ] Build `/in` landing page with Hindi + INR + Razorpay mention (QW12)
- [ ] Add Stripe Payment Link placeholder in Settings → Support development
- [ ] Add Sync waitlist email capture (localStorage `clauk_sync_waitlist`)
- [ ] Wire `navigator.storage.persist()` request on first save
- [ ] Implement smart auto-categorization heuristics (regex/keyword detection on paste)
- [ ] Implement Continue conversation prompt wrapping
- [ ] Implement streak counter (V1.1)
- [ ] Implement daily reflection prompt (V1.1)
- [ ] Build comparison pages (`/vs/mempalace`, `/vs/supermemory`, `/vs/ai-context-flow`, `/vs/mem0`) for Week 2 SEO
- [ ] Draft Week 2 launch posts (Reddit India, ClaudeAI, PH, HN, X thread, LinkedIn)

## V1.1 (Week 2-3) backlog
- [ ] Streak counter UI + logic
- [ ] Daily reflection prompt
- [ ] Browser extension preparation work
- [ ] Full Hindi translation (replace stub)

## V1.2 (Month 1-2) backlog
- [ ] Browser extension (Manifest V3, activeTab)
- [ ] ZIP import for ChatGPT / Claude / Gemini export bundles
- [ ] Stripe Payment Links active (not placeholder)
- [ ] Razorpay Payment Pages active
- [ ] License verification mechanism (JWT or lookup)
- [ ] Webhooks if conversion rate justifies

## V2 (Month 6+) backlog
- [ ] Tauri desktop wrapper with clipboard watcher
- [ ] Cloud sync ($3/mo optional)
- [ ] University partnerships (verified `.edu` priority)
- [ ] Foundation grant applications (Anthropic, OpenAI, Mozilla, Linux Foundation)

---

# Open Questions

## Architectural
1. Should free LLM proxy run on Cloudflare Workers (current plan) or fall back to Deno Deploy / Fly.io if Cloudflare rate-limits become a blocker?
2. License verification for paid tier — signed JWT (offline-verifiable, no backend lookup) vs lookup endpoint (revocable but requires uptime)?
3. Should ZIP import (V1.2) live client-side (privacy-pure but resource-heavy on large exports) or via Cloudflare Worker (faster but adds network step)?

## Operational
4. Domain choice: `clauk.app` (modern, app-store-y), `clauk.dev` (developer-vibe), `clauk.io` (classic, expensive)?
5. GitHub org: `clauk-app` (clean), `vit-clauk` (personal-attached), personal account (simplest)?
6. Hindi translation: founder DIY (cheap, risky), hire freelancer ($50–200), use machine translation + native review ($30–80)?
7. Razorpay KYC timing — start this week (early but blocks until done) or after Week 1 launch confirms demand (slower but capital-efficient)?

## Product
8. Founder's Edition cap — 500 (current, scarcity-strong) or 1,000 (more revenue, less exclusive)?
9. Should V1 include the `/vs/X` comparison pages or defer to Week 2? (Trade-off: SEO start time vs Day 7 scope creep)
10. Demo video — single horizontal master + repurpose vertical (more work but covers all channels) or vertical-only TikTok-style (one-channel optimized)?
11. Should free Gemini quota reset at midnight UTC (simple) or per-user-local-midnight (fair but complex)?

## Strategic
12. When/whether to apply for university partnership programs (Anthropic Builder Grants, OpenAI Academic, Google for Education)?
13. Should we publicly publish revenue numbers monthly (transparency-builds-trust play) or stay private (standard)?
14. At what milestone do we open a second product line (e.g., Clauk for Teams) vs deepen Clauk's current single-product focus?

---

# Known Bugs and Issues

## Prototype-level (as of 12 May 2026) `[inferred]`

| Issue | Severity | Likely cause | Mitigation |
|---|---|---|---|
| Prototype is in inconsistent intermediate state | Medium | Pivot mid-edit on 12 May; hero/onboard partially student-first, rest of UI pre-pivot | Decide rebuild-vs-continue before Day 1 |
| iOS PWA storage eviction after 7 days inactivity | High (existential) | Apple platform constraint | D-2901-2903 stack: install overlay + dual-write localStorage + weekly toast |
| Free Gemini quota abuse risk | High | If user count > rate-limit thresholds, service breaks for all | Per-fingerprint cap (50/day) + per-IP cap (100/hr) + multi-provider fallback |
| IndexedDB write failures on private browsing | Medium | Some browsers throw on IndexedDB in private/incognito | Detect and warn user; fall back to in-memory session |
| Web Share Target permissions on iOS | Low | Inconsistent iOS support | Feature-detect; show alternative paste-from-clipboard guide |

## V1.2+ anticipated `[inferred from architecture]`
- Stripe webhook delivery to Cloudflare Worker — need queue for retry on failure
- Razorpay KYC rejection paths — what happens if Vit's KYC isn't approved? (Stripe-only fallback)
- License key migration if user replaces device — currently no story

---

# Technical Debt

## Knowing in advance (will accumulate as V1 ships)

| Item | When it bites | How to clean |
|---|---|---|
| No build system | V1.2 when adding browser extension (probably needs Vite/esbuild) | Add minimal Vite config; preserve no-framework constraint |
| No automated tests | V1 build week | Add Playwright for E2E on Day 7 if time permits; otherwise V1.1 |
| No CI/CD | V1 build week | Manual `wrangler publish` to Cloudflare; add GitHub Actions in V1.1 |
| Inline CSS/JS in single HTML | When file exceeds ~250 KB | Split into modules; preserve single-deploy convention |
| `t()` function with inline `data-i18n` attributes | When string count > ~150 | Continue pattern; only refactor if it slows builds |
| Vanilla JS state management (no library) | When app state branches > ~10 views | Consider lightweight (Nano Stores ~1 KB, Apache 2.0) — never React |
| Manual deep-link table (currently 7 entries) | When LLM count > 15 | Move to JSON config; keep in-repo |
| Card schema migration story | First breaking change | Add `schemaVersion` to card object now (cheap insurance) |

## Carried forward from original May 2 PRD (intentionally deferred)
- Accessibility-tree parsing for capture (was F2 in original PRD; not needed for current paste-first model)
- OCR fallback for hard-to-copy text (was in original Smart Harvest spec; defer to V2+)
- AI-powered title generation, summary generation, topic tagging (probabilistic features explicitly avoided in V1)
- Confidence metadata capture (the "capture confidence" concept from §11 of original PRD — relevant for V1.1 Open Threads context)

---

# Development Workflow

## Setup (target — to be documented during Day 1 by founder)

```bash
# Clone
git clone https://github.com/clauk-app/clauk
cd clauk

# No npm install needed for V1 — single HTML + vendored libs

# Local dev: any static server
python3 -m http.server 8000
# or
npx serve public

# Worker dev (for free LLM proxy)
cd worker
npm install -g wrangler
wrangler dev
```

## Testing

| Layer | Approach |
|---|---|
| Manual acceptance criteria | Walk through A1-A40 from `clauk-prd-v3.md` §9 |
| Lighthouse | Target Performance ≥ 95, Accessibility ≥ 95, Best Practices ≥ 95, SEO ≥ 90, PWA = installable |
| Cross-browser | Test on Chrome (desktop + Android), Safari (iOS + Mac), Firefox, Edge |
| Cross-device | iPhone 12+, Pixel 6+, mid-range Android (Realme/Redmi), iPad |
| Storage durability | Use Chrome DevTools to revoke persistence, verify dual-write rescue |
| Web Share Target | Receive share from any Android app; verify `?share=1` URL handler |
| Privacy claim verification | Charles Proxy or browser DevTools Network panel — confirm 0 requests during capture/export |

## Deployment (V1 day 7)

```bash
# PWA static
wrangler pages deploy public --project-name=clauk

# Worker (free LLM proxy)
cd worker
wrangler deploy

# Custom domain
# Cloudflare Dashboard → Pages → Custom domains → add clauk.app
```

## Branching strategy `[inferred — to be confirmed]`
- `main` — always deployable
- Feature branches optional (solo founder; commit-to-main is acceptable for V1)
- Release tags: `v1.0`, `v1.1`, `v1.2` etc.
- No git-flow overhead for solo founder

## Release process
1. Bump version in `manifest.json` and inline `<meta name="version">`
2. Update README if user-facing change
3. Commit + tag
4. `wrangler pages deploy public`
5. Smoke-test on iPhone + Android + desktop
6. Tweet release notes if material

---

# Environment and Infrastructure

## Production (V1)

| Component | Provider | Tier | Estimated cost |
|---|---|---|---|
| Static PWA hosting | Cloudflare Pages | Free | $0 |
| Free LLM proxy | Cloudflare Workers | Free → paid | $0 → $5/mo |
| Domain | Cloudflare Registrar | At-cost | ~$10–15/yr |
| Payment processor | Stripe | Pay-per-transaction | 2.9% + $0.30 |
| Payment processor (India) | Razorpay | Pay-per-transaction | 2% + ₹3 |
| Email (waitlist export) | None V1; manual export | $0 | $0 |
| Email (transactional) | Cloudflare Email (workers) | Free | $0 |
| Analytics | None | $0 | $0 |
| Monitoring | Cloudflare native | Free | $0 |
| **Total V1 burn** | | | **~$5–10/mo** |

## Free LLM API tiers (downstream)

| Provider | Free tier | Notes |
|---|---|---|
| Google AI Studio (Gemini) | 1,500 requests/day on Gemini 2.5 Pro / Flash | Free tier in some regions; verify per-country |
| Groq (Llama 3.3 70B) | 14,400 requests/day | Most generous free tier; primary fallback |
| OpenRouter `:free` models | Rate-limited, model-dependent | Tertiary fallback |
| Cloudflare Workers AI | Free Llama/Mistral models | Quaternary fallback (built into Cloudflare; zero-config) |

## Secrets handling `[evidence from D-3411]`
- Stripe API keys live in Cloudflare Worker environment variables, never in client code
- Razorpay merchant keys same approach
- User-provided BYOK API keys (Gemini/Groq/OpenAI/Anthropic): encrypted in localStorage using Web Crypto with a key derived from device fingerprint; never leave device
- No central secrets vault needed (no shared infrastructure beyond the one Worker)

## Databases
- IndexedDB (client-side) is the only "database" in V1
- No PostgreSQL / MySQL / MongoDB / Supabase / Firebase
- V2 cloud sync may introduce Cloudflare D1 — explicitly deferred

---

# Coding Standards and Conventions

## From UI/UX Skill Pack `[evidence from /mnt/project/ui-skill]`

- **Compound components, never boolean props.** `<Button.Primary>` not `<Button isPrimary>`.
- **Tokens only, no raw hex/px.** Use CSS variables defined in `:root`.
- **Mobile-first.** Base design at 375px. Scale up.
- **4/8 spacing rhythm.** All spacing from `--space-N` scale.
- **One signature accent.** Don't compete colors.
- **Vector icons only (Lucide-style inline SVG).** No emoji as functional UI.
- **Animations: 150–300ms, transform/opacity only.** No `width/height/top/left` animations.
- **Touch targets: 44×44 pt minimum.** WCAG 2.1 AA contrast floor.
- **All text uses Inter (Google Fonts).** Mono = JetBrains Mono for technical chrome.

## Vanilla JS conventions

- **Single HTML file with inline `<script>` and `<style>`** for V1 (D-3415)
- Function naming: `camelCase`, action-first verbs (`saveCard`, `openCapture`, `renderList`)
- No classes unless lifecycle demands; prefer modules of pure functions
- All async via `async/await`, never raw Promise chains
- `try/catch` around every IndexedDB operation
- `data-i18n="key"` attribute on every user-facing string element; `t(key)` lookup function reads from locale JSON

## Card object shape (canonical)

```javascript
{
  id: "uuid-v4",
  type: "Bio" | "Role" | "Project" | "Goal" | "Decision" | "Discovery" | "Preference" | "Style" | "Person" | "Snippet",
  title: "string",
  content: "markdown string",
  tags: ["string"],
  scope: "personal" | "work" | "study" | "general",
  source: "claude" | "chatgpt" | "gemini" | "perplexity" | "grok" | "manual" | "imported",
  status?: "pending" | "resolved" | "abandoned",        // Decision cards only
  confidence?: "hypothesis" | "validated" | "invalidated", // Discovery cards only
  ts_created: 1234567890,
  ts_updated: 1234567890,
  ts_last_used: 1234567890,
  use_count: 0,
  schemaVersion: 1
}
```

## Privacy conventions (enforced)

- Every outbound fetch must show a Privacy Verifier counter increment
- Every feature that touches network must have a "Privacy" label in its UI explaining what leaves the device
- Free LLM passthrough must show pre-flight confirmation on first use
- Settings → Privacy Verifier panel is canonical truth source — its claims must match runtime behavior

---

# Important Assumptions

## Market assumptions
1. The multi-AI usage pattern (using ChatGPT + Claude + Gemini together) is permanent and growing — `[evidence: Mem0 State of Memory report, Plurality 200-hour-loss claim, Manthan Patel LinkedIn 173 comments]`
2. Students in India + UK + USA can afford ₹2,000 / £19 / $24 per year for a tool they use weekly — `[inferred from lunch-cost comparison; needs validation]`
3. Privacy-conscious users will choose local-first over cloud-first even at lower feature parity — `[evidence: MemPalace 19.5K stars, Screenpipe traction]`
4. PWA install rate on iOS will be > 25% with proper install overlay — `[inferred; iOS PWA install rate is industry-soft data]`
5. Free Gemini quota will not be revoked by Google in V1 timeframe (6 months) — `[uncertain — depends on Google policy]`

## Technical assumptions
6. IndexedDB available + reliable on target devices — `[evidence: 96%+ browser support per caniuse.com]`
7. Web Crypto API AES-GCM is sufficient for current threat model (data-at-rest on user's own device) — `[evidence: NIST-approved cipher]`
8. Cloudflare Workers free tier (100 K req/day) sustains until ~1,000 active users — `[inferred from 50-call/day cap × 1,000 users = 50K/day]`
9. Single HTML file approach scales to ~200 KB before performance suffers — `[inferred from Lighthouse data]`
10. Manifest V3 browser extension permission model survives Chrome/Edge policy churn — `[uncertain — Google has shipped MV3 changes]`

## Business assumptions
11. Solo founder can sustain 7-day intensive build week — `[evidence: prior conversation discipline; subjective]`
12. Word-of-mouth distribution in student communities outperforms paid marketing — `[evidence: Notion, Figma, Linear case studies; unverified for AI memory category]`
13. Founder's Edition at 500 units will sell out within 12 months — `[inferred — needs launch-week data]`

## Compliance assumptions
14. Zero-data architecture satisfies GDPR/DPDPA/CCPA without certification or DPA — `[strongly supported by legal precedent; unverified by lawyer]`
15. Free LLM passthrough disclosure (D-3412) is sufficient for downstream-policy applicability — `[uncertain — depends on jurisdiction interpretation]`

---

# Historical Mistakes and Lessons Learned

## From within the project history

| Lesson | Source | When learned |
|---|---|---|
| **Accessibility-tree parsing for capture is too fragile across AI apps that update weekly** | Original May 2 PRD's F2 feature | Rounds 1–5 of strategy conversation |
| **React Native cross-platform "saves time" claim doesn't survive contact with single-founder reality** | Considered & rejected | Round 6–10 |
| **MCP server integration is dev-only audience; mobile users don't have Claude Desktop installed** | Considered & deferred | Round 10–14 |
| **Free-forever-for-everything is a marketing trap; selects users who'd never pay anyway** | Investor verdict + revenue model rethink | Round 27–28 |
| **Login bridge / session injection is forever-rejected: browser security model + ToS + user-hostile** | Round 24 (D-2905) | 12 May |
| **Trying to out-feature Comet ($21B, 700 engineers) is the wrong strategy; refusing that lane IS the moat** | Competitive scan synthesis | Round 26 (D-3001-3002) |
| **Surveillance-based memory (Rewind, Limitless, Mirix) is the wrong product category to compete in** | Rewind→Meta cautionary tale analysis | Round 26 |
| **The "data monetization" line in original May 2 PRD was a trust killer; correctly removed in v2** | PRD evolution | Round 23–25 |

## Anti-patterns to never re-introduce

- Adding cookies "just for analytics"
- Adding `prompt-as-service` features that require server-side LLM inference (costs spiral)
- Switching to subscription on core features under revenue pressure
- Trading privacy for growth-hacking (referral popups, push-notification spam, etc.)
- Adding emoji as functional UI elements (violates ui-skill conventions)
- Re-introducing React or Vue or Svelte in V1
- Adding "AI title generation" or "AI summary generation" in V1 (probabilistic features explicitly excluded — see original PRD §12)

---

# Rejected or Abandoned Approaches

## Architectural rejections

| Approach | Why rejected | When |
|---|---|---|
| Native iOS + Android apps in V1 | App Store 30% cut + review delays + dual codebase | Round 6–10 |
| React Native | Adds 150–200 KB bundle for solo founder pace; PWA covers 85–95% UX | Round 6–10 |
| MCP server in V1 | Dev-only audience; mobile users don't have Claude Desktop | Round 10–14 |
| Accessibility-tree parsing | Fragile across AI app UI updates; Android-only viable | Round 1–5 |
| Screen capture / OCR fallback for "hard-to-copy text" (original F-list) | Surveillance posture conflicts with trust moat | Round 21–24 |
| Cloud-first sync as V1 feature | Adds infrastructure burn + compliance burden + trust risk | Multiple rounds |
| Login bridge / cookie-injection automation | Browser security + ToS + user-hostile | Round 24 |
| Browser-agent automation (Comet's lane) | $21B competitor; can't out-build | Round 26 |
| Passive-capture wearable / Rewind-style screen recorder | Surveillance + Meta-acquired cautionary tale | Round 26 |

## Product rejections

| Approach | Why rejected | When |
|---|---|---|
| Always-on background capture | Original "Smart Harvest" autonomous variant — abandoned in favor of user-triggered only | Round 1–5 |
| Anonymous data monetization (training-data play) | Trust killer; explicitly removed from PRD | Round 23 |
| Subscription on core features | Violates D-401 promise | Round 27–28 |
| Ads on free tier | Violates trust positioning | Multiple |
| App Store distribution | 30% cut + review delays + native-app overhead | Round 8 |
| AI-powered title/summary/tag generation in V1 | Probabilistic outputs add complexity without proven value | Round 28 |
| Universal AI memory API (developer-first product) | Wrong audience for indie founder + Mem0 already dominates | Round 14 |
| Charging from Week 1 with full subscription stack | Pivoted to lunch-money lifetime-favored model | Round 29 |
| "Free forever for everything, never monetize" | Investor-verdict math doesn't sustain | Round 27 |

## Pricing rejections

| Pricing | Why rejected | When |
|---|---|---|
| $9/mo Sync + $19/mo Pro (early plan) | Too high for student target | Round 28 |
| Per-token billing (Supermemory model) | Wrong audience; punishes power users | Round 16 |
| Single global $24/yr (no regional pricing) | Excludes Indian volume opportunity | Round 30 |
| $49 lifetime (initial proposal) | Lunch-money frame demands ~$29 | Round 30 |
| Lifetime cap of 1,000+ | Dilutes scarcity; 500 is sweet spot | Round 30 |
| No free tier (paid-only) | Kills word-of-mouth | Multiple |

---

# Important Terminology and Glossary

| Term | Definition |
|---|---|
| **Clauk** | The product name. Pronunciation undefined; "clauck" likely. Domain target: clauk.app |
| **Smart Harvest** | Original (May 2 PRD) feature concept for guided capture from AI apps via accessibility tree + OCR. Superseded by paste + Web Share Target. |
| **Context card** | The atomic unit of Clauk's vault. One of 10 typed cards (Bio, Role, Project, etc.). |
| **`cm.md`** | Clauk-native Markdown export format with YAML frontmatter. Designed to be the canonical portable file. |
| **`AGENTS.md`** | Linux Foundation universal standard for AI agent context. Clauk co-exports this with `cm.md`. |
| **Switzerland positioning** | The "neutral memory layer across walled-garden AI apps" framing inherited from Plurality's vocabulary. Used in investor decks. |
| **Lunch-money pricing** | $24/yr · £19/yr · ₹2,000/yr · $29 lifetime — pricing band designed to be impulse-purchase, not investment-purchase. |
| **Founder's Edition** | The 500-spot lifetime tier. Closes permanently after 500 units sold. Scarcity-driven. |
| **Open Threads** | Filter chip showing Decisions with status=pending + Discoveries with confidence=hypothesis. Brain-mcp-inspired UX. |
| **Card of the Day** | Daily re-engagement mechanism: surfaces one random old card with "still relevant?" prompt. |
| **Daily-return monopoly** | Three-layer retention design (mechanical / re-engagement / habit) to drive 25-30% DAU/MAU. |
| **Migrate-friendly** | Marketing principle: leaving Clauk is one click. Trust through portability. |
| **The five legitimate bridges** | D-2906: deep links · Web Share Target · browser extension · BYOK API · free LLM passthrough. Replaces the rejected login-bridge concept. |
| **The three promises** | D-401: free for core forever · stays on device · works in any AI |
| **The 7-check protocol** | UI-skill quality gate: a11y, style-match, boolean-props, tokens, motion, mobile-first, reduced-motion+dark-mode |
| **The 10-priority rule framework** | UI-skill conflict resolver: lower number wins (a11y > touch > perf > style) |
| **Vit** | Founder pseudonym used throughout PRD authorship |
| **Path A / Path B** | Strategic forks from investor verdict: A = independent / lifestyle business · B = make it investable. Path A locked. |
| **PWA** | Progressive Web App. Clauk's chosen distribution vehicle. |
| **D-XXXX** | Decision identifier convention. D-101 through D-3422 currently in use (~102 total). |
| **QW1–QW12** | Day 6 quick wins (V1 polish features). |
| **A1–A40** | Acceptance criteria identifiers in PRD v3 §9. |
| **BYOK** | Bring Your Own Key — for users with their own paid Gemini/OpenAI/Anthropic API keys. |
| **Sand-West** | Reddit user whose Karabiner F5 macro for "summarize and paste" prompt is cited as smoking-gun pre-validation. |

---

# Pending Features and Future Roadmap

## V1.1 (Week 2-3 post-launch)
- Streak counter UI + logic
- Daily reflection prompt
- Smart auto-categorization on paste (regex/heuristics)
- Continue conversation prompt wrapping
- Open Threads filter chip wired to real schema
- Hindi full translation
- Public comparison pages (`/vs/X`)
- Browser extension preparation (manifest, content-script skeleton)

## V1.2 (Month 1-2)
- Browser extension shipped (Chromium Manifest V3)
- ZIP import for ChatGPT / Claude / Gemini export bundles
- Stripe Payment Links active
- Razorpay Payment Pages active
- Lemon Squeezy fallback for EU
- License verification (JWT or lookup)
- Sync waitlist email export pipeline

## V2 (Month 6+)
- Tauri desktop wrapper with clipboard watcher
- Cloud sync ($3/mo optional, E2E encrypted, BYOK)
- "Ask AI" tab — V2 brain-mcp-inspired retrieval over user vault
- University partnerships (verified `.edu` priority)
- Foundation grant applications

## Year 2 sustainability options
- Teams tier ($12/seat) for hackathons + startup teams
- Enterprise edition (self-hosted, paid)
- Optional cloud LLM tier (BYOK to user's own paid Claude/GPT key)
- Native iOS + Android apps if PWA retention hits ceiling

## Explicitly never on roadmap
- Subscription on core features
- Ads
- Data sale or licensing
- Screen capture
- Browser-agent task automation
- Login-bridge integration

---

# Session Continuity Notes

## Read these first (in order) when picking up this project

1. **`clauk-prd-v3.md`** — binding scope doc; ~28 KB; cancels anything in v1/v2 it disagrees with
2. **This file (`MASTER_CONTEXT.md`)** — full chronology + rejected paths + glossary
3. **`/mnt/project/ui-skill`** — design system; tokens and conventions are non-negotiable
4. **`/mnt/project/C_PRD_2_May`** — original problem statement; useful for understanding ancestor decisions (especially the §11 "preserve raw + cleaned + final" data discipline)
5. **`clauk-prototype.html`** — current visual + functional reference; **note state inconsistency** — partial pivot mid-edit

## What context matters most for future LLMs

- **Clauk is independent, not VC-backed.** Path A is locked. Suggesting investor-friendly compromises is out of scope.
- **Refusal IS the moat.** Suggesting features that involve surveillance, cloud-first, browser-agent automation, login bridge, or per-token billing will be rejected on sight.
- **Multi-region pricing is structural, not cosmetic.** India isn't an afterthought; it's the primary market. UK + USA are secondary.
- **MIT/Apache-only stack is non-negotiable.** Verify license on every new dep.
- **Solo founder pace.** All scope decisions assume 40 hrs/week max. Suggesting "just add tests for everything" violates scope discipline.
- **The original May 2 PRD's "Smart Harvest" capture concept is HISTORICAL only.** Current capture model is paste + Web Share Target + browser extension (V1.2). Do not re-introduce accessibility-tree parsing.

## Where confusion is likely

- **Prototype vs PRD v3 mismatch:** prototype reflects pre-pivot state in places. Trust PRD v3 over prototype.
- **PRD v1 vs PRD v2 vs PRD v3:** v3 is canonical. v1 and v2 are historical context only.
- **D-XXXX numbering:** sequential by introduction date, not by feature area. To find a decision, search the full text of PRD v3 §5.
- **"Free forever for what matters" vs "free forever for everything":** the former is the locked V3 framing (D-3105). Earlier framing was looser; the V3 framing is the law.
- **Founder's Edition cap timing:** 500 is current (locked but with open question whether to expand to 1,000). Do not assume infinite.
- **iOS PWA storage:** Apple evicts after 7 days inactivity unless installed. This is existential. D-2901-2903 stack is mandatory.

## What must not be changed casually

- The three promises (D-401)
- Card type list (10 types)
- Export format list (6 formats, `cm.md` + `AGENTS.md` priority)
- Cookie-free architecture
- Anonymous-first / no-account
- Refusal list (D-2606, D-2905, D-3002 — permanent rejections)
- Free Gemini quota cap (50/day) — abuse = service breakage for all users
- Founder's Edition 500-cap (currently locked, open question for adjustment)

## What can be iterated freely

- UI copy (within ui-skill tone)
- Specific quick-win features (subset of QW1-QW12 is acceptable if Day 6 runs over)
- Order of capture surfaces beyond #1-4 (the V1 four are locked)
- Marketing channel selection beyond the V1 week-2 plan
- Comparison page list (`/vs/X` selection)

---

# Recommended Next Actions

## Immediate (this week — pre-Monday)

1. **Founder writes "PRD v3 locked, build starts Monday"** in conversation or repo — closes scope debate
2. **Register domain** (recommend `clauk.app` — modern, app-store-y, easy to spell)
3. **Create GitHub org** (recommend `clauk-app` — clean, separates personal from project)
4. **Start Razorpay KYC** — longest dependency (3-7 days); start now, parallel-track to V1 build
5. **Decide rebuild-from-PRD-v3 vs continue-from-prototype**
   - **Recommendation:** Rebuild from PRD v3. Prototype is in intermediate state; rebuild is cleaner and faster than reconciling mid-pivot file.
6. **Generate Day 1 Claude Code build prompt** that includes:
   - PRD v3 §6 feature scope
   - ui-skill design tokens
   - Card schema (from §6 of this doc)
   - File structure (from this doc § Repository and File Structure)

## Week 1 (build)

Follow PRD v3 §8 sequence exactly. Do not reorder.

## Week 2 (launch)

Follow PRD v3 §13 launch plan. Sequence: Reddit India + Reddit ClaudeAI → Product Hunt → Hacker News → X thread → LinkedIn.

## Strategic decisions to make in next 30 days

- Founder's Edition cap (500 vs 1,000) — wait for launch-week signal
- Whether to publicly publish monthly revenue (transparency play)
- University partnership outreach timing
- Hindi translation quality strategy (DIY vs hire)
- Demo video format (single horizontal + repurpose, or vertical-only)

## Strategic decisions to make in next 90 days

- Browser extension rollout sequence (V1.2 vs deferred)
- Whether to build Teams tier (Year 2 product expansion)
- Whether to apply for foundation grants (Anthropic, OpenAI, Mozilla)
- Whether to formalize lifestyle-business legal entity (LLC in USA, Pvt Ltd in India, etc.)

---

**End of MASTER_CONTEXT.md. This document is intended to be self-sufficient. If any future LLM finds it ambiguous, escalate by asking the human to clarify rather than guessing.**
