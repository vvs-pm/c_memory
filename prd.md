# Clauk · PRD v3 — Student-First, Multi-Region, Frontend-First

**Version:** 3.0
**Date:** 12 May 2026
**Owner:** Vit
**Status:** Locked for V1 build
**Supersedes:** PRD v1 (May 2), PRD v2 (May 12 morning)

---

## 0. Why This PRD Exists

PRD v2 locked the student-first pivot and lunch-money pricing. This PRD v3 expands that to a **multi-region audience** (India primary, UK + USA secondary), adds **legal-compliance scope**, locks the **MIT-only technology stack**, and produces a **frontend-first scope** so we can ship maximum value without backend investment in V1.

This document is the canonical reference for the 7-day V1 build. Everything outside this PRD is research notes; everything inside is binding scope.

---

## 1. Product Identity (Locked)

| Attribute | Value |
|---|---|
| **Name** | Clauk |
| **Tagline** | Free AI memory + free AI for students. Built on your phone. |
| **Core idea (D-3001)** | The structured memory layer for people who'd rather not have an AI watching them |
| **Domain** | clauk.app (to register Day 7) |
| **License** | MIT |
| **Distribution** | PWA at clauk.app · no app stores in V1 |
| **Pricing model** | Free forever for core · Clauk One Year ₹2,000 / $24 / £19 · Founder's Edition ₹2,500 / $29 / £23 lifetime (500-cap) |
| **Primary user** | Students + builders in India, UK, USA, EU who cannot afford $720/yr for ChatGPT + Claude + Gemini |

---

## 2. The Audience (Locked, Refined for V3)

### Primary persona — "Aryan, Computer Science student in Bangalore"
- 21, BTech 3rd year, building side projects + interview prep
- Uses ChatGPT free + Gemini free + Claude free in parallel
- Has ~₹500-2,000/month discretionary spend
- **Willing to pay ₹2,000/year ($24)** for tools that compound knowledge
- Mobile-first (Android, mid-range phone, often on 4G)
- Privacy-aware but not paranoid
- Shares tools with classmates via WhatsApp + Discord

### Secondary persona — "Maya, second-year PhD in Manchester"
- 26, social sciences research, hates academic Notion/Mendeley overhead
- Uses Claude for literature review, ChatGPT for drafting
- Subscription budget: £10-30/month max
- **Willing to pay £19/year** for cross-LLM memory
- iPhone, often on shared university Wi-Fi
- GDPR-aware, reads privacy policies

### Tertiary persona — "Marco, indie hacker in Austin"
- 32, building 3rd SaaS attempt while consulting
- Uses Claude Code daily, GPT-4 for marketing copy, Perplexity for research
- Already pays $60/mo across AI tools, wants to consolidate
- **Willing to pay $29 lifetime / $24/year**
- Mac + iPhone, technical, expects open-source

### Excluded for V1
- Enterprise teams (Year 2)
- Pure non-technical creators (need higher-level UX investment)
- LLM agent developers (different lane — that's Mem0 / Supermemory)

---

## 3. The Problem (Quantified)

### The cost problem (the new core pain)

| Tool | Annual cost (US) | Annual cost (UK) | Annual cost (India) |
|---|---|---|---|
| ChatGPT Plus | $240 | £192 | ₹19,990 (~$240) |
| Claude Pro | $240 | £192 | ₹19,990 (~$240) |
| Gemini Advanced | $240 | £192 | ₹19,490 (~$235) |
| Total if running all three | **$720** | **£576** | **₹59,470 (~$715)** |
| AI Context Flow / Plurality Pro | $240 | £192 | ₹19,990 |
| Memory tool subscription | +$60-228 | +£48-180 | +₹4,800-18,000 |
| **Grand total for full multi-AI memory stack** | **$780-948** | **£624-756** | **₹64,000-78,000** |

For an Indian computer science student earning ₹0 (or stipend ₹15,000/month), **₹64,000/year for AI tools is approximately their entire entertainment + food budget for 6 months.** Unaffordable.

### Clauk's offering

| Clauk tier | India | UK | USA | What it gives |
|---|---|---|---|---|
| Free forever | ₹0 | £0 | $0 | All capture + export + 10 card types + 50 free Gemini/day + 500 free Groq/day |
| Clauk One Year | **₹2,000** | **£19** | **$24** | + priority quota + future paid features for 12 months |
| Founder's Edition | **₹2,500** | **£23** | **$29** | Lifetime everything · 500-spot cap · supports indie development |

**One lunch in India ≈ ₹250-400. One lunch in UK ≈ £10-15. One lunch in USA ≈ $15-20.** Annual Clauk = roughly 6-8 lunches. Lifetime = 8-10 lunches.

### Validated user voices

| Source | Quote | Date |
|---|---|---|
| r/ClaudeAI | "I made this prompt template to deal with conversation length limits. Please steal it..." | Mar 2025 |
| r/ClaudeAI | "I made a prompt to fully summarize the chat and made a Karabiner shortcut using F5 to paste it" | Dec 2024 |
| LinkedIn (Manthan Patel, 173 comments) | "I hate switching between ChatGPT and Claude because it forgets my entire project context" | Oct 2025 |
| Plurality blog | "Switching between AI tools costs professionals 200+ hours annually" | Jan 2026 |
| Mem0 State of AI Agent Memory report | Industry-validated: "memory is the #1 unresolved problem in 2026" | Apr 2026 |

The pain is real, quantified, and getting worse as more AI tools launch.

---

## 4. Locked Decisions Carried Over From PRD v2

All 40+ D-decisions from PRD v2 remain in force. New decisions in this PRD start at **D-3401**.

Key carried decisions:

| # | Decision |
|---|---|
| D-401 | Three promises: no paywall on free features · stays on device · works in any AI |
| D-2003 | Export defaults: `cm.md` (Clauk-native) + `AGENTS.md` (universal) |
| D-2207 | 6 capture surfaces by Month 6 |
| D-2601 | Use `idb` library for IndexedDB |
| D-2901-2906 | iOS install overlay + 5 legitimate bridges, no login bridge |
| D-3001-3003 | Core idea + audience filter + auto-categorization |
| D-3107 | Steal-and-refuse master list from 13 competitors |
| D-3201-3206 | Student-first pivot + free Gemini passthrough Day 6 |
| D-3301-3304 | Lunch-money pricing + 3-layer daily-return monopoly + migrate-friendly trust moat |

---

## 5. New Decisions Locked In This PRD (D-3401 through D-3422)

### Multi-region positioning

| # | Decision |
|---|---|
| **D-3401** | **Primary market: India.** Pricing in INR (₹2,000/yr, ₹2,500 lifetime). Localized copy on /in landing page. UPI + Razorpay as payment options alongside Stripe. |
| **D-3402** | **Secondary markets: UK + USA.** Pricing in GBP/USD. Same product, different currency. Stripe for both. |
| **D-3403** | **Tertiary markets: EU (DE, FR, ES, PT) + Brazil + Indonesia + Vietnam + Philippines.** Reachable via PWA + locale detection. No paid tier in these markets V1; revisit Year 2 based on signal. |
| **D-3404** | **i18n stub locales in V1**: `en`, `hi` (Hindi for India), `es` (Spanish), `pt-BR` (Brazilian Portuguese). Full translation deferred to V1.1 based on traffic data. |
| **D-3405** | **Pricing displayed in user's local currency** using `navigator.language` detection. Manual override in Settings. |

### Compliance scope

| # | Decision |
|---|---|
| **D-3406** | **Compliance posture: local-first means most compliance is automatic.** No personal data leaves device → most GDPR/DPDPA/state-law obligations are mooted. |
| **D-3407** | **GDPR (EU + UK)**: Privacy policy page at /privacy explicitly states "we collect zero personal data." Cookie banner not required (we use no cookies). Right-to-delete = built-in Wipe button. |
| **D-3408** | **DPDPA (India, Digital Personal Data Protection Act 2023)**: Same as GDPR — zero data collection makes most provisions inapplicable. India-specific privacy note on /in/privacy page. |
| **D-3409** | **CCPA (California + state-level US)**: Same logic. Privacy policy adds California section. No data sale (we don't sell anything). |
| **D-3410** | **Cookie-free architecture**: Clauk uses zero cookies. Only `localStorage` + `IndexedDB`. This is a structural advantage — no consent banner needed in EU/UK. |
| **D-3411** | **Payment data handled by Stripe/Razorpay only.** Clauk never sees card numbers, names, or addresses. Reduces compliance burden to zero. |
| **D-3412** | **Free LLM passthrough disclosure**: When user uses free Gemini, UI explicitly says "Your question goes to Google AI Studio. Their privacy policy applies to that request." Honest, legal, defensible. |
| **D-3413** | **No telemetry / analytics in V1.** Page views, error logs, usage stats — all off. Add only if needed in V1.2 with explicit opt-in. |

### Technology stack (MIT/OSS only)

| # | Decision |
|---|---|
| **D-3414** | **Locked stack — all MIT or Apache 2.0**: HTML/CSS/Vanilla JS (no framework V1) · `idb` (MIT, Jake Archibald) · Web Crypto API (native) · Cloudflare Workers (free tier) · Cloudflare D1 (V2) · Tauri (V2 desktop) · `Fuse.js` (Apache 2.0, search) · `marked.js` (MIT, Markdown rendering). **No AGPL. No GPL. No proprietary deps.** |
| **D-3415** | **No build system in V1.** Single HTML file with inline JS/CSS, hosted directly on Cloudflare Pages. Build system added V1.1 if complexity demands. |
| **D-3416** | **No framework in V1.** Vanilla JS only. Adopting React/Vue/Svelte costs ~150KB bundle + complexity that V1 doesn't need. Revisit V2.0 if features demand it. |
| **D-3417** | **No backend in V1 core features.** Capture, store, export, search, deep-link all run client-side. Backend (Cloudflare Worker) only for the free LLM proxy. |
| **D-3418** | **Free LLM proxy on Cloudflare Worker** ($5/mo, 100K req/day free): aggregates Gemini AI Studio (1,500/day) + Groq Llama 3.3 (14,400/day) + OpenRouter free models. Rate-limited per-fingerprint (50 Gemini calls/day, 500 Groq/day). Zero user logs. |
| **D-3419** | **Open-source from Day 7.** GitHub repo public with MIT license. README links to live PWA. Hosts comparison pages. Drives SEO via amnesia + ai-memory tags. |
| **D-3420** | **Self-hostable as a feature.** Anyone can fork the HTML, host on their own domain. Builds trust through forkability. Marketed as: "Don't trust us? Run it yourself." |

### Frontend-first scope

| # | Decision |
|---|---|
| **D-3421** | **Frontend-first scope locked for V1.** Everything that can be client-side IS client-side. Backend reserved exclusively for the free LLM proxy. This keeps burn at $5/mo and complexity minimal. |
| **D-3422** | **Stripe Payment Links + Razorpay Payment Pages for V1 monetization.** No payment backend needed. Both services issue hosted checkout URLs. Clauk only redirects to them. Webhooks (for license verification) added V1.2 only if free→paid conversion proves material. |

---

## 6. The V1 Feature Scope (Final, Locked)

### 6.1 Section 1 — Trust Foundation (Day 1-2)

| Feature | Detail |
|---|---|
| **Local IndexedDB via `idb`** | Hardware-encrypted vault. AES-GCM 256. Key derived via Web Crypto on this device. |
| **Privacy Verifier panel** | Live fetch/XHR interceptor counter. Persistent storage status. Share Target registered. Encryption status. Last export. |
| **Multi-format export** | `cm.md` · `AGENTS.md` · `CLAUDE.md` · `GEMINI.md` · `.cursor/rules/*.mdc` · JSON |
| **Wipe all data** with DELETE-typed confirmation | Destructive action protected |

### 6.2 Section 2 — Cold-Start Unlock (Day 3)

| Feature | Detail |
|---|---|
| **Curated 10-card starter pack** | With `{{variables}}` baked in. Bio · Role · Project · Goal · Style · Preference examples. |
| **Three-promise hero + onboarding** | Updated to student-first framing per D-3001 |
| **First-launch empty state** | No demo data. User must install pack or capture. |

### 6.3 Section 3 — Capture Loop (Day 4)

| Feature | Detail |
|---|---|
| **Paste-from-clipboard button** with permission explainer | One-tap from any source |
| **{{variables}} fill-on-copy** with live preview | Variables: {name}, {role}, {project}, {style} etc. |
| **Smart auto-categorization on paste** | Regex/heuristics infer card type, source LLM, suggested title, suggested tags. Pure client-side. |
| **Quick-edit on copy** with monospace + variable highlighting | Final review before export |

### 6.4 Section 4 — Reuse Loop (Day 5)

| Feature | Detail |
|---|---|
| **Smart launchpad** with verified deep-links + clipboard fallback | Claude / ChatGPT / Perplexity auto-prefill. Gemini / Grok / Le Chat open + paste guide. |
| **Continue conversation mode** | Toggle between "New question" and "Continue chat 🔄" wrapping |
| **"Ask Clauk's free AIs"** | Buttons: Ask Gemini (50/day free) · Ask Groq Llama (500/day free). Backend deferred — UI ships with placeholder. |

### 6.5 Section 5 — Habit Layer (Day 5)

| Feature | Detail |
|---|---|
| **Smart Reminders via Service Worker** | Opt-in per card with "next fire" preview |
| **Open Threads filter chip** on home | Surfaces Decisions with status=pending, Discoveries with confidence=hypothesis |
| **Card of the Day** | Surfaces one random old card daily for "still relevant?" prompt |
| **Streak counter** | Days-of-capture streak. Light gamification. |

### 6.6 Section 6 — Day 6 Polish + Quick Wins

| QW | Feature |
|---|---|
| **QW1** | `navigator.storage.persist()` request on first save |
| **QW2** | Web Share Target API in manifest + `?share=1` handler |
| **QW3** | Multi-format export modal (6 formats) |
| **QW4** | Persistent storage status row in Privacy Verifier |
| **QW5** | Share Target registered status in Privacy Verifier |
| **QW6** | iOS install overlay + "open weekly" educational toast |
| **QW7** | Free Gemini + Groq passthrough UI in launchpad (backend Cloudflare Worker stubbed but not deployed in V1 Day 7) |
| **QW8** | AI Access Settings section with quota display + BYOK key entry |
| **QW9** | Lunch-money pricing teaser in Settings ("Coming Week 4") — no purchase button yet, just waitlist email capture |
| **QW10** | Multi-currency display (₹/£/$) based on `navigator.language` |
| **QW11** | `/privacy` page with GDPR + DPDPA + CCPA sections |
| **QW12** | `/in` landing page with INR pricing + Hindi tagline + UPI/Razorpay mention |

### 6.7 Section 7 — Day 7 Ship

| Task | Detail |
|---|---|
| Final QA against 25+ acceptance criteria | A1-A31 from PRD v2 + new A32-A40 |
| Deploy to Cloudflare Pages | Free tier hosts entire PWA |
| GitHub repo public with MIT license | clauk-app/clauk |
| 60-sec demo video | Shows capture → export → reuse loop |
| Reddit / PH / HN launch drafts ready | For Week 2 |

---

## 7. The Locked Tech Stack (One Page)

| Layer | Choice | License | Why |
|---|---|---|---|
| **Hosting** | Cloudflare Pages | Free | Global edge, zero cost, instant deploy |
| **Markup/Style/Logic** | HTML5 + CSS3 + Vanilla ES2022 | Open | No framework overhead. Single-file shipping. |
| **Storage primary** | IndexedDB via `idb` (Jake Archibald) | MIT | 8KB Promise wrapper. Industry standard. |
| **Storage backup** | localStorage (last 50 cards on iOS) | Native | iOS IndexedDB instability safety net |
| **Encryption** | Web Crypto API (AES-GCM 256) | Native | Built into every modern browser |
| **PWA shell** | Web App Manifest + Service Worker | Native | Install-to-home-screen on iOS/Android |
| **Share capture** | Web Share Target API | Native | OS-level share sheet integration |
| **Persistence guarantee** | `navigator.storage.persist()` | Native | Protects vault from eviction |
| **i18n** | Custom `t()` function + JSON locale files | MIT (our code) | Lightweight, no library overhead |
| **Search** | `Fuse.js` (when card count > 100) | Apache 2.0 | Fuzzy search, 14KB |
| **Markdown rendering** | `marked.js` | MIT | Preview rendering |
| **Free LLM proxy** (V1 Day 7) | Cloudflare Worker (free tier) | — | $5/mo cap; aggregates Gemini + Groq + OpenRouter |
| **Payment processor** | Stripe Payment Links + Razorpay Payment Pages (V1.2) | — | No backend; hosted checkout URLs |
| **Analytics** | None (V1) — opt-in privacy-respecting only V1.2+ | — | D-3413 |
| **Desktop wrapper** (V2 Month 6) | Tauri v2 + tauri-plugin-clipboard | MIT/Apache 2.0 | ~3MB native binary |
| **Browser extension** (V1.2 Month 2) | Manifest V3 with `activeTab` only | — | Privacy-preserving, Chrome/Edge/Firefox |

**Total V1 bundle target:** under 80KB gzipped. Currently ~135KB raw (prototype) which compresses to ~35KB gzipped — well under target.

**Zero AGPL, zero GPL, zero proprietary dependencies.** All MIT or Apache 2.0 or native browser APIs.

---

## 8. The 7-Day Build Sequence (Final)

| Day | Focus | Hours |
|---|---|---|
| **D1 Mon** | Project scaffold · `idb` setup · IndexedDB schema · AES-GCM encryption · file structure | 6 |
| **D2 Tue** | Capture flow · paste button · permission explainer · smart auto-categorization heuristics · save to vault | 6 |
| **D3 Wed** | Card list · search · filters · scopes · Card of the Day · Open Threads chip · streak counter scaffold | 6 |
| **D4 Thu** | Launchpad · deep-link table · Continue conversation mode · Ask Clauk's free AIs UI · BYOK key entry | 6 |
| **D5 Fri** | Settings · Privacy Verifier (real fetch interceptor) · AI Access section · multi-currency · privacy page | 6 |
| **D6 Sat** | All 12 quick wins (QW1-QW12) · iOS install overlay · Web Share Target · multi-format export · `/in` landing | 5 |
| **D7 Sun** | QA against acceptance criteria · Lighthouse 95+ · deploy to Cloudflare Pages · GitHub repo public · demo video | 5 |

**Total: 40 hours over 7 days.** Solo-founder pace.

---

## 9. Acceptance Criteria (V3 — Expanded from PRD v2)

Carry forward all A1-A31 from PRD v2, plus:

| # | Criteria | Test |
|---|---|---|
| A32 | INR/USD/GBP currency displays correctly based on locale | Set `navigator.language='hi-IN'` → see ₹2,000 · `en-GB` → £19 · `en-US` → $24 |
| A33 | `/privacy` page lists GDPR, DPDPA, CCPA sections | Open `/privacy` → all 3 sections present, each ≤200 words |
| A34 | `/in` landing page exists with Hindi tagline | Open `/in` → Hindi heading present + INR pricing |
| A35 | Cookie-free verified | DevTools → Application → Cookies → 0 entries |
| A36 | Card of the Day surfaces a random old card daily | Open home → card section shows one card with "still relevant?" prompt |
| A37 | Open Threads chip shows correct count | Filter chip shows count = (Decisions where status=pending) + (Discoveries where confidence=hypothesis) |
| A38 | Streak counter increments on first capture of new day | Save card → if no card saved yesterday, streak=1; if card saved yesterday, streak +=1 |
| A39 | Free Gemini quota UI displays 50/50 starting count | Settings → AI Access → "Free Gemini: 50/50 today" |
| A40 | Continue conversation mode wraps prompt correctly | Toggle to Continue → tap Open in Claude → clipboard contains: "Here's a conversation I was having with [source]. Continue from where it left off. My next question: " |

---

## 10. The Reuse Strategy (Master Steal List from 13 Competitors)

Already locked in PRD v2 (D-3107). Reproduced here for V1 build reference:

| What to STEAL | Source | Where in Clauk V1 |
|---|---|---|
| Comparison-page playbook (`/vs/X` pages) | MemPalace | Day 8+ marketing |
| "Single portable file" framing | MemVid | `cm.md` marketing |
| Memory Buckets UI concept | Plurality AI Context Flow | `scope` field as "Buckets" |
| Lifetime deal financing model | myNeutron | Founder's Edition $29 |
| Cognitive prosthetic UX (status / confidence) | brain-mcp | Decision + Discovery card fields |
| Startup credit program concept | Supermemory | "Founders Circle" Year 2 |
| 25-tool specialized search design | brain-mcp | V2 "Ask AI" tab |
| Universal AI memory positioning | Plurality | Switzerland-for-AI investor pitch |
| AGENTS.md universal standard | Linux Foundation | Export format |

### What to REFUSE (anti-patterns)

| What to REFUSE | Why | Permanent? |
|---|---|---|
| Per-token billing | Supermemory model — wrong audience | Permanent |
| Cloud-first architecture | Plurality / Mem0 / etc — wrong moat | Permanent |
| Screen capture / passive surveillance | Mirix / Rewind — destroys trust | Permanent |
| Browser-agent automation | Comet's $21B lane — can't compete | Permanent |
| VC pressure dynamic | Rewind sold to Meta cautionary tale | Permanent |
| Medical metaphors (AI Doctor) | Regulatory risk + brand collision | Permanent |
| Login bridge / session hijacking | Browser security + ToS violation | Permanent |
| App Store distribution V1 | 30% cut + review delays | V1 only |
| Native mobile app V1 | PWA gives 85-95% UX at 5% effort | V1-V2 only |
| Account requirement | Local-first means none needed | Permanent |

---

## 11. The Compliance Map (Multi-Region)

| Region | Law | Clauk's posture | What we add to `/privacy` |
|---|---|---|---|
| **EU + UK** | GDPR + UK-GDPR | Zero data collection. Right-to-delete = built-in. No cookie banner needed. | GDPR section: list of data we don't collect + Wipe button reference |
| **India** | DPDPA 2023 | Zero data collection. Cross-border transfer rules don't apply (no transfer). | DPDPA section: explicit statement of nil collection + free LLM passthrough notice |
| **California, USA** | CCPA + CPRA | No sale of data (we don't have data). | CCPA section: right-to-know + right-to-delete (both automatic) |
| **Brazil** | LGPD | Zero data collection. | LGPD section: minimal — refer to GDPR section |
| **Other** | Various | Same posture | Generic "we don't collect data" + jurisdiction-aware Wipe button |

**Critical compliance insight:** Local-first architecture is the strongest possible compliance posture. We don't have to comply with most data laws because we don't collect data. The /privacy page is mostly a list of what we don't do, plus the free LLM passthrough disclosure (Gemini/Groq do receive queries during AI calls — their privacy policies apply).

---

## 12. Pricing & Monetization (Final V3)

### Pricing tiers (locked)

| Tier | India (INR) | UK (GBP) | USA (USD) | EU (EUR) | What it unlocks |
|---|---|---|---|---|---|
| **Free Forever** | ₹0 | £0 | $0 | €0 | All capture · All export · 10 card types · 50/day free Gemini · 500/day free Groq · Unlimited cards on device |
| **Clauk One Year** | ₹2,000 | £19 | $24 | €22 | + 200/day priority Gemini · 1,500/day priority Groq · All future paid features for 12 months |
| **Founder's Edition** (500 cap) | ₹2,500 | £23 | $29 | €27 | Lifetime everything · Name in credits · Direct line to founder · Supports indie development |

### Revenue forecast (V3, conservative)

| Month | Free users | One Year sold | Lifetime sold | Cumulative revenue |
|---|---|---|---|---|
| M1 | 500 | 30 (₹60K / $720) | 20 (₹50K / $580) | ~$1,300 |
| M2 | 1,500 | 60 | 25 | ~$2,500 |
| M3 | 3,500 | 100 | 30 | ~$4,000 |
| M6 | 12,000 | 350 | 100 | ~$10,000 |
| M9 | 25,000 | 600 | 200 | ~$17,500 |
| M12 | 50,000 | 1,200 | 500 (closed) | **~$30,000** |

**Honest math:** Multi-region pricing makes the target more achievable. India provides volume, UK/USA provide ARPU. Combined, $30K Year 1 with solo founder + $5/mo burn is a real indie business.

### Payment infrastructure

| Region | Primary | Fallback |
|---|---|---|
| India | Razorpay Payment Pages (UPI + cards) | Stripe |
| UK / EU | Stripe Payment Links | Lemon Squeezy (handles EU VAT automatically) |
| USA | Stripe Payment Links | — |
| Other | Stripe Payment Links | — |

All Payment Links generated in respective dashboards. **No payment backend in V1.** Webhooks added V1.2 only if conversion rates justify.

---

## 13. Distribution & Launch Plan (Final V3)

### Week 1 (V1 build) — D1-D7
Build V1 per the 7-day plan. Stay heads-down.

### Week 2 (V1 launch) — D8-D14

| Day | Channel | Post angle |
|---|---|---|
| D8 Mon | Reddit r/India · r/SideProject | "Built free AI memory for Indian students. ₹2,000/yr Founder's Edition or use forever free. Open source. AMA." |
| D9 Tue | Reddit r/ClaudeAI · r/ChatGPTPro | "I solved my own multi-AI context problem — a free local-first memory app. Lifetime $29." |
| D10 Wed | Product Hunt | "Clauk — Free AI memory + free Gemini for students" |
| D11 Thu | Hacker News | "Show HN: Clauk — local-first AI memory PWA. ₹2,000/yr full access or use forever free." |
| D12 Fri | X/Twitter thread | Demo video + the multi-region price comparison |
| D13 Sat | LinkedIn (Indian B2B/edu audience) | "How students in India can save ₹60K/year on AI tools" |
| D14 Sun | Settle in. Respond to feedback. Patch bugs. |

### Week 3 (V1.1) — Sync waitlist conversion + improvements
### Week 4-8 (V1.2) — Browser extension + ZIP import + paid tier hard launch
### Month 3-6 — University outreach in India + UK
### Year 2 — Sustainability + scale decisions

---

## 14. Risk Register (V3)

| Risk | Probability | Impact | Mitigation |
|---|---|---|---|
| **Free Gemini quota abuse** | High | Service breakage | Per-fingerprint rate limit + multi-provider fallback (Groq, OpenRouter) |
| **Native LLMs ship full feature parity in 6 months** | Medium | Reduced wedge | Multi-region pricing + free Gemini access maintains differentiation |
| **MemPalace pivots to mobile** | Low | Direct competitor | Stay independent, ship faster, own the cheap-mobile-student lane |
| **iOS PWA storage eviction** | Medium | User data loss | `storage.persist()` + dual-write localStorage + iOS install overlay |
| **Razorpay onboarding delay (KYC)** | Medium | India payment block | Launch with Stripe-only, add Razorpay Month 2 |
| **Compliance scrutiny in EU** | Low | Forced re-architecture | Zero-data architecture already compliant; `/privacy` page in place |
| **Solo founder burnout** | High | Product stagnation | Sustainable indie pace · accept smaller scope · no VC pressure |
| **Cost overrun on Cloudflare** | Low | $5/mo → $50/mo | Hard rate limits + manual review at 1K daily users |

---

## 15. The Single One-Line Verdict

> **Clauk is a free-forever, local-first AI memory PWA for students and beginner builders who can't afford $720/year for multiple AI subscriptions. Lunch money ($24/yr or $29 lifetime) unlocks priority access. MIT-licensed, multi-region, compliance-clean by architecture.**

That's the product. Everything else is execution.

---

## 16. Open Questions for Tomorrow

| # | Question | Owner |
|---|---|---|
| 1 | Domain registration: clauk.app vs clauk.dev vs clauk.io — which? | Vit, Day 7 |
| 2 | Razorpay merchant account setup timing (KYC takes 3-7 days in India) — start now or after Week 1? | Vit, this week |
| 3 | Cloudflare Worker free LLM proxy — Vit to deploy or use Claude Code? | Both |
| 4 | Hindi translation for `/in` page — DIY or hire? | Vit, Day 6 |
| 5 | GitHub org name: clauk-app vs vit-clauk vs personal? | Vit, Day 6 |
| 6 | License verification (paid tier) — V1.2 implementation: signed JWT or lookup endpoint? | TBD V1.2 design session |
| 7 | Should Founder's Edition cap be 500 (current) or 1,000 (more revenue, less exclusive)? | Vit, before Week 2 launch |
| 8 | Demo video format: 60s vertical (TikTok/Reels) or 90s horizontal (YouTube/X)? | Both, Day 7 |

---

## 17. Memory for Tomorrow

Pick up tomorrow at:

1. **Approve PRD v3 in writing** — confirm "locked, build starts Day 1 Monday"
2. **Resolve Open Question 1 + 5** (domain + GitHub org) — both blocking Day 7 publish
3. **Start Razorpay KYC** (Open Question 2) — 3-7 day delay is the longest dependency
4. **Generate Day 1 Claude Code build prompt** — ready to feed Claude Code Monday morning
5. **Choose whether to ship Hero rewrite + AI Access section to existing prototype now** — partial-step option vs full V1 rebuild

---

## 18. Decision Log Index (Cumulative)

PRD v1 locked decisions: D-101 through D-1903 (~30 decisions)
PRD v2 locked decisions: D-2001 through D-3304 (~50 decisions added)
PRD v3 locked decisions: **D-3401 through D-3422** (22 new decisions added)

**Total locked decisions: ~102 across the project.**

Every decision has a rationale. No decision is mystery. All decisions trace to either user pain validation, competitor analysis, technical constraint, legal requirement, or sustainable-indie-business math.

---

## 19. The Conversation That Built This PRD

This PRD is the product of a 17-round, ~20,000-token strategy conversation that scanned:
- 13+ competitors (MemPalace, Mem0, Supermemory, Plurality, myNeutron, brain-mcp, MemVid, OpenMemory, Rewind/Limitless, Screenpipe, Mirix, Comet, AI Context Flow)
- 8+ user-voice sources (Reddit, LinkedIn, Mem0 industry report, Plurality blog)
- 5+ technical reference patterns (idb, Tauri, Cloudflare, Web Share Target, PWA limitations)
- Multi-round validation of pricing, positioning, audience, compliance, technology

This is not aspirational. This is binding scope. Build it.

---

**End of PRD v3. Locked 12 May 2026.**
