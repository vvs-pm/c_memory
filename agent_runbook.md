# AGENT_RUNBOOK.md — Agentic AI SDLC for Clauk V1

> **Purpose:** Executable runbook. Any agentic AI (Claude Code, Cursor Agents, Windsurf, Devin, OpenAI Codex, Replit Agent) follows this to deliver Clauk V1 end-to-end with minimal human intervention.
> **Scope:** Full SDLC — discovery validation → design → architecture → build → test → deploy → launch.
> **Time budget:** 7 calendar days · 40 founder hours · ~120 agent hours
> **Author:** Vit + Claude (PRD v3 + MASTER_CONTEXT.md as ground truth)
> **Last updated:** 21 May 2026

---

## 0. How to Use This Runbook

### For the human (founder)
1. Pick one agentic AI (recommended: Claude Code or Cursor Agents — most mature for this stack).
2. Feed it: `MASTER_CONTEXT.md` + `clauk-prd-v3.md` + this runbook (`AGENT_RUNBOOK.md`).
3. Run **Phase 0 (Pre-flight)** manually — must complete before any code runs.
4. For each phase below, paste the **Agent Prompt** block into the agent, then verify the **Acceptance Gate** before moving on.
5. Never skip a gate. The gates exist because each phase has irreversible side effects.

### For the agent
- Each phase has: `Goal · Inputs · Outputs · Steps · Verification · Failure Recovery · Time Box`
- You must **stop and report** if any verification fails. Do not proceed past a failed gate.
- You must **NEVER**: commit secrets, push to main without verification, deploy without smoke test, use an AGPL/GPL dep, introduce a framework not in the locked stack.
- You must **ALWAYS**: cite the D-decision number when implementing a locked decision (e.g., "Implementing per D-3202"), log every tool call to `agent.log`, and request human confirmation for any irreversible action (deploy, payment-link generation, domain DNS change).

### Locked stack reminder (D-3414 through D-3422)
- HTML + CSS + Vanilla JS (no framework)
- `idb` for IndexedDB
- Cloudflare Pages + Workers
- All deps MIT or Apache 2.0
- No build system V1
- No backend except free LLM proxy

---

## Phase 0 — Pre-flight (Day 0, ~4 hours, HUMAN-LED)

### 0.1 Goal
Verify project context is loaded, tooling is installed, secrets are provisioned, and human is ready to supervise.

### 0.2 Inputs required
- [ ] `MASTER_CONTEXT.md` read end-to-end
- [ ] `clauk-prd-v3.md` read end-to-end
- [ ] `ui-skill` design system tokens reviewed
- [ ] Domain registered (clauk.app / .dev / .io)
- [ ] GitHub org created
- [ ] Cloudflare account exists
- [ ] Razorpay KYC submitted (3–7 day delay → start NOW)
- [ ] Stripe account active

### 0.3 Tooling install
```bash
# Required versions
node --version    # ≥ 20
npm --version     # ≥ 10
git --version     # any recent
wrangler --version  # ≥ 3.0 — install if missing: npm i -g wrangler

# Verify
wrangler whoami   # must show logged-in Cloudflare account
gh auth status    # if using GitHub CLI
```

### 0.4 Secrets checklist (provision before Day 1)
| Secret | Where it goes | Source |
|---|---|---|
| `CLOUDFLARE_API_TOKEN` | Local env + GH Actions | Cloudflare dashboard → My Profile → API Tokens |
| `GEMINI_API_KEY` | Cloudflare Worker env var | aistudio.google.com → Get API key |
| `GROQ_API_KEY` | Cloudflare Worker env var | console.groq.com → API Keys |
| `OPENROUTER_API_KEY` | Cloudflare Worker env var | openrouter.ai/keys |
| Stripe publishable + secret | Stripe dashboard only (no client code) | dashboard.stripe.com |
| Razorpay key_id + key_secret | Razorpay dashboard only | dashboard.razorpay.com (post-KYC) |

**Rule:** Secrets never appear in client code, never get committed, never get logged. Use `wrangler secret put` for Worker env.

### 0.5 Phase 0 verification gate
- [ ] All tooling installed and authenticated
- [ ] All required secrets provisioned (or KYC pending, documented)
- [ ] Repo skeleton initialized: `git init && gh repo create clauk-app/clauk --public --license=mit`
- [ ] PRD v3 + MASTER_CONTEXT.md + this runbook committed to `main`
- [ ] Human-readable handoff message to agent: "All pre-flight complete, proceed to Phase 1"

### 0.6 Failure recovery
If KYC is the blocker: launch Stripe-only first, add Razorpay in Week 2. Document this divergence in `decisions.log`.

---

## Phase 1 — Discovery Validation (Day 1 AM, ~3 hours, AGENT-LED with human spot-check)

### 1.1 Goal
Re-verify PRD v3 assumptions against the current web state. Catch any breaking changes (free LLM tier revocations, competitor launches, regulatory shifts) before committing 7 days of build.

### 1.2 Inputs
- `clauk-prd-v3.md` (especially §3 quantified pain, §10 steal-and-refuse, §14 risk register)
- `MASTER_CONTEXT.md` § Important Assumptions

### 1.3 Agent prompt block
```
ROLE: You are a discovery-validation agent for the Clauk project.

CONTEXT: Read MASTER_CONTEXT.md and clauk-prd-v3.md fully before starting.

TASK: Re-verify these 12 facts as of today's date. Web-search each one. Report any that have changed.

  1. Google AI Studio still offers free Gemini tier with ≥1,500 req/day on Gemini 2.5 Pro or Flash
  2. Groq still offers free Llama 3.3 70B tier with ≥14,400 req/day
  3. OpenRouter still has :free model tier
  4. Cloudflare Workers free tier is still 100K req/day
  5. Cloudflare Pages free tier still hosts unlimited static sites
  6. Web Share Target API still supported on Android Chrome
  7. iOS Safari PWA storage eviction is still ~7 days after last use
  8. ChatGPT Plus, Claude Pro, Gemini Advanced still ~$20/mo each
  9. Razorpay Payment Pages still no-code (no backend required)
  10. Stripe Payment Links still no-code
  11. No major competitor has shipped Clauk's exact combination (mobile + local + multi-LLM + free Gemini bundled + lunch-money pricing) since 12 May 2026
  12. EU GDPR + India DPDPA 2023 + California CCPA all still in force with no breaking amendments for zero-data-collection products

DELIVERABLE: Append findings to `validation-log.md`:
  - Each fact: VERIFIED / CHANGED / UNVERIFIABLE
  - For CHANGED: cite source URL + describe the change + recommend impact

STOP CONDITIONS:
  - Any of facts 1, 2, 8, 11 has CHANGED → escalate to human BEFORE Phase 2
  - 3 or more facts UNVERIFIABLE → escalate to human

OUTPUT FORMAT: clean markdown, one section per fact, no fluff.
```

### 1.4 Acceptance gate
- [ ] `validation-log.md` exists and covers all 12 facts
- [ ] No CHANGED status on facts 1, 2, 8, 11 (or human-approved deviation logged)
- [ ] Findings reviewed by human

### 1.5 Failure recovery
- If Gemini free tier revoked → pivot D-3202: use Groq + OpenRouter only; update pricing if needed
- If competitor shipped same wedge → review §10 steal list and check whether differentiation still holds
- If GDPR/DPDPA amended → consult lawyer before launch (delays V1)

### 1.6 Time box: 3 hours. If unfinished, escalate.

---

## Phase 2 — Architecture Lock (Day 1 PM, ~2 hours, AGENT-LED)

### 2.1 Goal
Convert PRD v3 §6-7 into a precise technical architecture document with explicit file structure, schemas, and API contracts.

### 2.2 Inputs
- `clauk-prd-v3.md` §6 (V1 feature scope) and §7 (locked tech stack)
- `MASTER_CONTEXT.md` § Architecture and § Coding Standards
- `ui-skill` (design system constraints)

### 2.3 Agent prompt block
```
ROLE: Solution architect for Clauk V1.

CONSTRAINTS (NON-NEGOTIABLE):
  - All deps MIT or Apache 2.0 (per D-3414)
  - No framework, no build system in V1 (per D-3415, D-3416)
  - Single HTML file architecture (per D-3417)
  - No backend except Cloudflare Worker LLM proxy (per D-3421)
  - Frontend-first scope (per D-3422)

TASK: Produce `ARCHITECTURE.md` containing:

  1. File-and-folder tree (target Git repo)
  2. IndexedDB schema (object stores, indices, version migration plan)
  3. localStorage key map (all keys + value shapes)
  4. Service Worker registration + Web Share Target manifest
  5. i18n key map (en.json structure; hi/es/pt-BR follow same shape)
  6. Cloudflare Worker API contract (endpoints, request/response shape, error codes, rate-limit response)
  7. State management approach (pure functions + module-level state; no library)
  8. Routing approach (single-page hash routes: #/home, #/list, #/settings, #/privacy)
  9. Encryption flow (key derivation, AES-GCM wrap/unwrap, key rotation strategy)
  10. CSS architecture (token-based, single style block, no preprocessor)

DO NOT: invent new card types, change export format list, introduce server-side state.

DELIVERABLE: ARCHITECTURE.md committed to repo root.

STOP CONDITIONS:
  - You find a PRD v3 decision that's technically infeasible → escalate with proposed alternatives
  - Any spec implies introducing AGPL/GPL/proprietary dep → escalate
```

### 2.4 Acceptance gate
- [ ] `ARCHITECTURE.md` covers all 10 required sections
- [ ] No introduced deps outside the locked stack
- [ ] IndexedDB schema matches canonical card object shape from MASTER_CONTEXT.md
- [ ] Cloudflare Worker API contract specifies rate-limit response format
- [ ] Human reviews and signs off

### 2.5 Failure recovery
If architect finds a feasibility blocker:
- Document in `decisions.log` as a deviation with rationale
- Propose smallest possible scope cut, not stack expansion
- Escalate to human for sign-off before proceeding

---

## Phase 3 — Build Day 1: Scaffold + Vault (~8 hours)

### 3.1 Goal
Project scaffold runs locally, IndexedDB works, AES-GCM encryption works, no UI yet.

### 3.2 Inputs
- `ARCHITECTURE.md` from Phase 2
- `MASTER_CONTEXT.md` § Coding Standards (canonical card schema)
- ui-skill tokens (paste-in CSS variables)

### 3.3 Agent prompt block
```
ROLE: Senior frontend engineer building Clauk V1 Day 1.

GOAL: Ship a working scaffold by end of session. UI is stub-only.

TASKS:
  1. Create `public/index.html` with:
     - HTML5 doctype, viewport meta, theme-color, manifest link
     - <style> block with full ui-skill CSS variables (--bg, --text, --brand, etc.)
     - <script type="module"> block with idb import via CDN (https://esm.sh/idb@8)
     - Minimal body: <main id="app">Loading...</main>

  2. Initialize IndexedDB:
     - Open database `clauk-vault` version 1
     - Object store `cards` with keyPath `id`, indices on `type`, `scope`, `ts_created`
     - Object store `meta` with keyPath `key`
     - Wrap all calls in try/catch
     - Log errors to console.error with [Clauk:DB] prefix

  3. Implement encryption helpers in inline <script>:
     - generateKey(): create or load device AES-GCM 256 key from IndexedDB `meta`
     - encrypt(plaintext): returns {ciphertext, iv}
     - decrypt({ciphertext, iv}): returns plaintext
     - Wrap card.content with encrypt() on save, decrypt() on read

  4. Implement saveCard(card) and loadAllCards() — for now, dump JSON to console

  5. Create `public/manifest.json`:
     - name "Clauk", short_name "Clauk", display "standalone"
     - start_url "/", scope "/"
     - theme_color, background_color from ui-skill tokens
     - icons 192 + 512 (use placeholder PNGs from a CC0 source for now)
     - share_target: action "/?share=1", method "GET", params {title: "title", text: "text", url: "url"}

  6. Create `public/sw.js`:
     - On install: skipWaiting
     - On activate: clients.claim
     - On fetch: network-first, fallback to cache for static assets

  7. Register service worker from index.html on load

  8. Smoke test:
     - Open in Chrome desktop
     - Save 3 cards via console (window.saveCard({...}))
     - Reload page
     - loadAllCards() returns 3 cards with decrypted content
     - DevTools → Application → Service Workers shows registered SW
     - DevTools → Application → IndexedDB shows clauk-vault populated

DELIVERABLE: All of public/*. Commit to `main` with message "Day 1: scaffold + vault + encryption".

DO NOT:
  - Add any framework (React, Vue, Svelte, jQuery, Lodash)
  - Add any UI beyond <main>Loading...</main>
  - Add any backend code
  - Add any analytics or telemetry

STOP CONDITIONS:
  - Smoke test fails → debug + retry; do not move on
  - Any dep license is not MIT/Apache → revert immediately

LOG: Append to agent.log: every file created, every key decision, any deviation from spec.
```

### 3.4 Acceptance gate
- [ ] `public/index.html` exists and loads in Chrome without errors
- [ ] IndexedDB `clauk-vault` visible in DevTools with `cards` and `meta` stores
- [ ] Manual save → reload → load cycle returns same encrypted data
- [ ] Service Worker registered (DevTools → Application → Service Workers)
- [ ] No console errors
- [ ] No deps outside MIT/Apache
- [ ] Lighthouse PWA section shows "installable" (may have warnings, that's fine for Day 1)

### 3.5 Failure recovery
- IndexedDB errors on Safari → check if Private Browsing mode; document
- Service Worker won't register → check serving over `localhost` or HTTPS (file:// won't work)
- AES-GCM key generation fails → verify Web Crypto availability (browser support)

### 3.6 Time box: 8 hours

---

## Phase 4 — Build Day 2: Capture Loop (~8 hours)

### 4.1 Goal
User can capture content via paste, smart auto-categorization fills 5 fields, user reviews and saves to encrypted vault.

### 4.2 Inputs
- `ARCHITECTURE.md`
- Day 1 scaffold
- PRD v3 §6.3 (Capture Loop section)

### 4.3 Agent prompt block
```
ROLE: Senior frontend engineer building Clauk V1 Day 2.

GOAL: Working capture loop — paste → auto-categorize → review → save → vault.

TASKS:
  1. Build the Capture sheet UI:
     - Modal/sheet from bottom on mobile, centered on desktop
     - Header: "Save context" + close icon
     - Body:
       * Paste button (uses navigator.clipboard.readText())
       * Textarea (auto-grows; min 4 lines, max 20 visible)
       * Title field (auto-filled by heuristic, user-editable)
       * Type selector (10 chips: Bio · Role · Project · Goal · Decision · Discovery · Preference · Style · Person · Snippet) — only one active
       * Source selector (chips: ChatGPT · Claude · Gemini · Perplexity · Manual · Other) — only one active
       * Tags field (comma-separated, auto-suggested top 3)
       * Confidence chip ("🤖 We're 78% sure this is a Decision — tap to change")
     - Footer: Cancel · Save (primary)

  2. Implement smart auto-categorization (pure regex/heuristics — D-3003):
     - Type detection:
       * /\\bI am a\\b|\\bI work as\\b|\\bMy role\\b/i → Bio
       * /\\bwe decided\\b|\\bchose\\b|\\bpicked\\b/i → Decision
       * /\\bI found\\b|\\blearned\\b|\\bdiscovered\\b/i → Discovery
       * /\\bTODO\\b|\\bplanning\\b|\\bgoal\\b/i → Goal
       * /\\balways\\b|\\bnever\\b|\\bprefer\\b/i → Preference / Style
       * Fall back to Snippet
     - Source detection:
       * "I'll help with that" / "Certainly!" preambles → Claude
       * Triple-backtick code blocks with specific syntax → Gemini
       * Default → Manual
     - Title: first sentence trimmed to 60 chars at word boundary
     - Tags: top 3 most-frequent meaningful words (filter stopwords)
     - Confidence: count regex matches / total possible matches → percentage

  3. Wire Save button:
     - Build card object matching canonical schema (from MASTER_CONTEXT.md)
     - Generate UUID v4 for id
     - Set ts_created = ts_updated = Date.now()
     - Call saveCard() from Day 1
     - Close sheet on success
     - Show toast "Saved" (use ui-skill toast pattern)

  4. Wire Cancel button:
     - If textarea has content, confirm before closing ("Discard this capture?")

  5. Add a fixed action button on home view: "Save first context" — opens Capture sheet

  6. Test cases (manual):
     - Paste a Claude response with "I'll help with that..." → source auto = Claude
     - Paste "We decided to use React over Vue because..." → type auto = Decision, confidence high
     - Paste empty content → Save button disabled
     - Save → reload → card visible in IndexedDB DevTools

DELIVERABLE: Commit to main: "Day 2: capture loop + smart auto-categorization".

DO NOT:
  - Use any AI inference for categorization (regex only per D-3003)
  - Add Tab key keyboard nav yet (Day 6 polish)
  - Add multi-card paste detection yet (V1.1)

STOP CONDITIONS:
  - Clipboard API blocked in current browser → show paste-into-textarea fallback
  - Regex categorization misclassifies > 30% of test cases → tune heuristics
```

### 4.4 Acceptance gate
- [ ] Capture sheet opens on tap, closes on cancel/save/backdrop
- [ ] Paste from clipboard fills textarea
- [ ] Auto-categorization fills type + source + title + tags + confidence
- [ ] User can override every auto-suggested field
- [ ] Save persists to encrypted IndexedDB
- [ ] Toast "Saved" appears on success
- [ ] No console errors

### 4.5 Time box: 8 hours

---

## Phase 5 — Build Day 3: List + Card of the Day + Open Threads (~8 hours)

### 5.1 Goal
Card list view works, daily-return mechanisms (Card of the Day + Open Threads chip + streak scaffold) functional.

### 5.2 Agent prompt block
```
ROLE: Senior frontend engineer, Clauk Day 3.

GOAL: Card list, search, Card of the Day, Open Threads chip, streak counter.

TASKS:
  1. Build List view (#/list):
     - Header: "Memory" · "X cards · stored on this device"
     - Search input (live-filter on title + content)
     - Filter chips row: All · Bio · Role · Project · Decision · Discovery · ... · "🔁 Open Threads (N)"
     - Sort dropdown: Recent · Oldest · Most Used · A-Z
     - Card list (virtualized only if count > 100 — use Fuse.js for fuzzy when > 50)
     - Each card row: type chip · title · first 2 lines of content · age · tag chips · more menu

  2. Implement Open Threads chip logic:
     - Count cards where:
       * type === "Decision" && status === "pending"
       * type === "Discovery" && confidence === "hypothesis"
       * type === "Project" && (Date.now() - ts_last_used) > 14 days
     - Display count in chip; clicking filters list to only these
     - Per D-3302 L2

  3. Implement Card of the Day on home view:
     - Pick deterministic random card based on date (so same card all day):
       * seed = Date.now() / (24 * 60 * 60 * 1000) | 0
       * Use seeded random to pick one card with use_count < 3 and age > 7 days
     - Display compact card with "Still relevant?" prompt + Yes/No buttons
     - Yes → use_count++, ts_last_used = Date.now()
     - No → archive flag (does not delete, just hides from Card-of-Day rotation)
     - Per D-3302 L2

  4. Implement Streak counter scaffold:
     - On card save: check if a card was saved yesterday (ts within 24-48 hrs)
     - If yes → streak++
     - If no → streak = 1
     - Persist to localStorage `clauk_streak`
     - Display on home view: "🔥 12 days"
     - V1.1 will add notifications + reflection prompts; for now just the counter

  5. Card detail modal:
     - Tap a card → opens detail modal
     - Shows full content (decrypted), all metadata, tags, source
     - Actions: Edit · Copy · Open in AI (placeholder for Day 4) · Delete
     - Delete requires confirmation ("Type DELETE to confirm")

  6. Wire i18n for all new strings:
     - Every user-facing string must have data-i18n attribute
     - Add keys to public/locales/en.json

DELIVERABLE: Commit "Day 3: list + daily return + streak scaffold".

STOP CONDITIONS:
  - Search performance > 200ms on 100-card vault → switch to Fuse.js immediately
  - Card of the Day shows same card across multiple users (test in incognito) → fix seeding
```

### 5.3 Acceptance gate
- [ ] List view renders all saved cards
- [ ] Search filters in real-time
- [ ] Open Threads chip shows accurate count + filters correctly when clicked
- [ ] Card of the Day surfaces a real card on home view
- [ ] Streak counter increments correctly on consecutive-day saves
- [ ] Detail modal shows full card with edit/delete

### 5.4 Time box: 8 hours

---

## Phase 6 — Build Day 4: Launchpad + Free AI Access (~8 hours)

### 6.1 Goal
User can open card in any AI tool (deep link or copy-paste guide), use Clauk's free Gemini/Groq via Cloudflare Worker proxy, or BYOK their own key.

### 6.2 Pre-requisite
Cloudflare Worker proxy must exist by end of this phase. Create it parallel to UI work.

### 6.3 Agent prompt block
```
ROLE: Senior frontend engineer + Cloudflare Worker engineer, Clauk Day 4.

GOAL: Launchpad (deep links + free AI passthrough + BYOK) working end-to-end.

PART A — Cloudflare Worker LLM proxy (worker/llm-proxy.js):

  1. Create wrangler.toml at repo root /worker:
       name = "clauk-llm-proxy"
       main = "llm-proxy.js"
       compatibility_date = "2026-01-01"

  2. Endpoints:
     - POST /ask
       Body: { provider: "gemini"|"groq"|"openrouter", prompt: string, fingerprint: string }
       Response: { text: string, provider: string, remaining: number }
     - GET /quota?fingerprint=X
       Response: { gemini: {used, max}, groq: {used, max} }

  3. Rate limiting:
     - Per-fingerprint daily cap: 50 Gemini, 500 Groq, 100 OpenRouter (reset midnight UTC)
     - Per-IP hourly: 100 reqs across all providers
     - Use Cloudflare Workers KV for counters (free tier)
     - Return 429 with { error, retryAfter } when exceeded

  4. Provider fallback chain on error:
     - If Gemini fails → try Groq
     - If Groq fails → try OpenRouter :free
     - If all fail → return 503

  5. Privacy:
     - Never log prompts to KV or external system
     - Log only: timestamp, fingerprint hash, provider, status code, response size (no content)
     - CORS: allow only clauk.app + localhost for dev

  6. Secrets via wrangler secret put:
     - GEMINI_API_KEY, GROQ_API_KEY, OPENROUTER_API_KEY

  7. Deploy to clauk-llm-proxy.<account>.workers.dev (custom domain later)

PART B — Frontend Launchpad UI:

  1. Build Launchpad section in card detail modal:
     - Two-mode toggle: "New question" (default) | "Continue chat 🔄"
     - Continue mode wraps copied content as:
       "Here's a conversation I was having with [source]. Continue from where it left off. My next question is: "
     - Per D-3206

  2. Section "Ask Clauk's free AIs":
     - Button: "Ask Gemini · 47/50 free today" (live quota from /quota endpoint)
     - Button: "Ask Groq Llama · 487/500 free today"
     - On click:
       a. Fetch /ask with current card content + question
       b. Show response in a sliding panel below
       c. Update quota display

  3. Section "Open in your own AI" (deep links):
     - Claude: https://claude.ai/new?q=<encoded>
     - ChatGPT: https://chatgpt.com/?q=<encoded>
     - Perplexity: https://www.perplexity.ai/search?q=<encoded>
     - Gemini: opens app + clipboard-paste guide (no working deep link)
     - Grok / Le Chat: same as Gemini

  4. Section "Use your own key (BYOK)":
     - Settings → AI Access has BYOK fields for: Gemini, Groq, OpenAI, Anthropic, OpenRouter
     - Keys encrypted with device AES-GCM, stored in IndexedDB `meta`
     - When BYOK key present for a provider, /ask call routes to user's key not Clauk's
     - Rate limit becomes the user's own (200/day Gemini if BYOK; 50 if proxy)

  5. Fingerprint generation (D-3205):
     - hash = SHA-256(navigator.userAgent + screen.width + screen.height + Intl.DateTimeFormat().resolvedOptions().timeZone)
     - Truncate to 16 chars
     - Persist to IndexedDB `meta.fingerprint`
     - Used only for proxy rate-limit lookup; never logged

DELIVERABLE: Commit "Day 4: launchpad + free AI proxy + BYOK".

STOP CONDITIONS:
  - Cloudflare Worker quota lookup > 1 second → optimize KV reads
  - Free Gemini API key blocked by Google → escalate (D-3202 risk)
  - BYOK encryption test fails on Safari → fix Web Crypto fallback
```

### 6.4 Acceptance gate
- [ ] Worker deployed and accessible
- [ ] "Ask Gemini" returns a real response in < 5 sec
- [ ] Quota decrements visible in UI on next call
- [ ] Rate limit returns 429 after 50 calls (test by manipulating fingerprint)
- [ ] BYOK Gemini key uses user's quota not Clauk's
- [ ] Deep links open the right AI in a new tab
- [ ] Continue conversation mode wraps prompt correctly
- [ ] No prompt content appears in any log

### 6.5 Time box: 8 hours

---

## Phase 7 — Build Day 5: Settings + Privacy Verifier + Multi-currency (~8 hours)

### 7.1 Agent prompt block
```
ROLE: Senior frontend engineer, Clauk Day 5.

GOAL: Settings page complete, Privacy Verifier proves zero-data claim, multi-currency display works.

TASKS:
  1. Settings sections (in order):
     - Privacy Verifier (real fetch interceptor)
     - AI Access (free quota + BYOK keys)
     - Data (export · install starter pack · wipe all)
     - Display (theme · language)
     - About (version · GitHub link · privacy link)

  2. Privacy Verifier panel:
     - Live counter: "0 outgoing network requests in this session" — uses fetch monkeypatch + XHR override
     - Color-coded badges:
       * Persistent storage: ✅ Granted / ⚠️ Not granted (request via navigator.storage.persist())
       * Share Target: ✅ Registered / ⚠️ Not registered
       * Encryption at rest: ✅ AES-GCM 256 (verify key in meta store)
       * Cookie-free: ✅ 0 cookies (verify document.cookie === "")
       * Last export: <timestamp> or "Never"
     - "Run audit" button — re-checks all and shows toast

  3. AI Access (per Day 4):
     - Free Gemini quota row (live from /quota)
     - Free Groq quota row
     - BYOK Gemini · BYOK Groq · BYOK OpenAI · BYOK Anthropic · BYOK OpenRouter
     - "What's free forever" info row at bottom

  4. Multi-currency display (D-3405):
     - On first load: detect locale via navigator.language
     - Map:
       * hi-IN, en-IN, mr-IN, ta-IN, te-IN, bn-IN → INR (₹)
       * en-GB, cy-GB, gd-GB → GBP (£)
       * pt-BR → BRL (R$)
       * es-ES, ca-ES → EUR (€)
       * Default → USD ($)
     - Manual override in Settings → Display → Currency
     - Persist to localStorage `clauk_currency`
     - Use across all price displays: Clauk One Year (₹2,000 / £19 / $24 / €22 / R$120), Founder's Edition (₹2,500 / £23 / $29 / €27 / R$150)

  5. Build /privacy.html page (QW11):
     - 5 sections: Overview · GDPR (EU + UK) · DPDPA (India) · CCPA (California, USA) · LGPD (Brazil)
     - Each section ≤ 200 words
     - Plain language, no legalese
     - Each says explicitly: "We don't collect [list]. Right-to-delete = the Wipe button in app."

  6. Build /in/index.html landing page (QW12):
     - Hindi tagline + English fallback
     - INR pricing prominently
     - Note: "UPI + cards via Razorpay coming Week 4"
     - Link to main app at /

DELIVERABLE: Commit "Day 5: settings + privacy verifier + multi-currency + /privacy + /in".

STOP CONDITIONS:
  - Fetch interceptor breaks app functionality → use Proxy() wrapper, not direct override
  - Locale detection wrong on test devices → log to console for debugging
```

### 7.2 Acceptance gate
- [ ] Privacy Verifier shows accurate live data
- [ ] Multi-currency: setting `navigator.language = 'hi-IN'` shows ₹ prices
- [ ] `/privacy` page exists with all 5 jurisdiction sections
- [ ] `/in` page exists with Hindi tagline + INR pricing
- [ ] All settings sections functional
- [ ] No console errors

### 7.3 Time box: 8 hours

---

## Phase 8 — Build Day 6: 12 Quick Wins + iOS Install Overlay (~7 hours)

### 8.1 Agent prompt block
```
ROLE: Senior frontend engineer, Clauk Day 6.

GOAL: All 12 quick wins shipped. iOS install overlay works. Lighthouse 95+ on all axes.

TASKS — implement ALL 12 quick wins in this order:

  QW1: navigator.storage.persist() — request on first card save; show toast result
  QW2: Web Share Target — verify manifest declaration + /?share=1 handler in JS
  QW3: Multi-format export modal — 6 formats (cm.md, AGENTS.md, CLAUDE.md, GEMINI.md, .mdc, JSON)
       - Each format generated client-side from vault
       - Download via Blob URL + a.click()
  QW4: Persistent storage status in Privacy Verifier (already in Day 5; verify)
  QW5: Share Target registered status (already in Day 5; verify)
  QW6: iOS install overlay (D-2901, D-2902, D-2903):
       - Detect iPhone/iPad UA AND not in standalone mode
       - Show overlay after user saves 2nd card:
         "Add Clauk to home screen so it doesn't disappear:
          1. Tap Share (square with up arrow)
          2. Scroll → Add to Home Screen
          3. Tap Add"
       - Include visual diagram (inline SVG)
       - Show "open weekly" toast on first save: "Apple removes uninstalled web apps after 7 days. Add to home screen to keep your vault safe."
       - Dual-write last 50 cards to localStorage on iOS for safety (D-2902)
  QW7: Free Gemini + Groq passthrough UI (already in Day 4 launchpad; verify polish)
  QW8: AI Access Settings section (already in Day 5; verify)
  QW9: Lunch-money pricing teaser in Settings (no payment button yet — V1.2):
       - "Coming Week 4: Clauk One Year ₹2,000 / £19 / $24"
       - "Founder's Edition ₹2,500 / £23 / $29 (500 only)"
       - "Get notified" → email capture to localStorage clauk_pricing_waitlist
  QW10: Multi-currency display (already in Day 5; verify across all price displays)
  QW11: /privacy page (already in Day 5; verify all 5 sections)
  QW12: /in landing page (already in Day 5; verify Hindi + INR)

POLISH TASKS:
  - WCAG AA contrast pass — use https://webaim.org/resources/contrastchecker/ values
  - Visible focus rings on all interactive elements
  - Keyboard nav: Tab cycles, Esc closes modals, Cmd/Ctrl+K opens search
  - Mobile keyboard handling (visualViewport API) — scroll editing field into view
  - Dark mode pairs verified
  - Animations all transform/opacity only (no width/height)
  - Reduced-motion media query respected
  - iOS Safari PWA install test passes

LIGHTHOUSE TARGETS (run on deployed preview, not localhost):
  - Performance ≥ 95
  - Accessibility ≥ 95
  - Best Practices ≥ 95
  - SEO ≥ 90
  - PWA: installable + all checks green

DELIVERABLE: Commit "Day 6: 12 quick wins + iOS install + Lighthouse 95+".

STOP CONDITIONS:
  - Lighthouse < 95 on Performance → optimize images, inline critical CSS, defer non-critical JS
  - iOS Safari rejects manifest → check share_target syntax
```

### 8.2 Acceptance gate
- [ ] All 12 QWs verifiably present
- [ ] Lighthouse run on deploy preview shows ≥95 on Performance, A11y, Best Practices
- [ ] iOS Safari install flow works (tested on real device)
- [ ] Dark mode tested in both Chrome and Safari
- [ ] Keyboard nav works without mouse

### 8.3 Time box: 7 hours

---

## Phase 9 — Build Day 7: QA + Deploy + GitHub Public + Demo Video (~7 hours)

### 9.1 Agent prompt block
```
ROLE: Release engineer + QA engineer + content creator, Clauk Day 7.

GOAL: V1 live at clauk.app, GitHub repo public, demo video published, ready for Week 2 launch.

PART A — QA (3 hours):
  1. Walk through all A1-A40 acceptance criteria from clauk-prd-v3.md §9
  2. For each: PASS / FAIL / WAIVER (with reason)
  3. Any FAIL on A1-A20 (core flow) blocks deploy
  4. FAIL on A21-A40 (polish) acceptable with documented deferral

PART B — Deploy (1 hour):
  1. Final build verification (no console errors, no broken links)
  2. Deploy static PWA:
     wrangler pages deploy public --project-name=clauk
  3. Deploy worker:
     cd worker && wrangler deploy
  4. Custom domains:
     - clauk.app → Pages
     - api.clauk.app → Worker
  5. Smoke test on production:
     - Save card → reload → load card
     - Ask Gemini → get response
     - Export cm.md → verify content
     - iOS install flow on real device
  6. If any smoke test fails → ROLLBACK via wrangler pages rollback

PART C — GitHub public (30 min):
  1. Verify .gitignore excludes: .env, .dev.vars, node_modules, .wrangler
  2. Verify no secrets in commit history (run git-secrets or trufflehog)
  3. Write README.md:
     - One-line product description
     - Live link
     - "What it does" (3 bullets)
     - "How to self-host" (5 steps)
     - License (MIT)
     - Link to MASTER_CONTEXT.md and clauk-prd-v3.md
  4. Push to clauk-app/clauk on GitHub
  5. Set repo to public
  6. Add topics: ai-memory, pwa, local-first, privacy, students
  7. Pin clauk.app in repo description

PART D — Demo video (2.5 hours):
  1. Format: 60s vertical (TikTok/Reels/Shorts) — recommended over horizontal for student reach
  2. Script (60s):
     - 0-5s: "Spending $720/year on ChatGPT + Claude + Gemini? You don't have to."
     - 5-15s: Show Clauk install on phone (iOS or Android)
     - 15-30s: Show capture → save → export → open in another AI
     - 30-45s: Show free Gemini access via Ask Clauk
     - 45-55s: Show migrate-friendly export to cm.md
     - 55-60s: "Clauk. Lunch money for a year of AI. Free forever for what matters."
  3. Tools: OBS or Screen Studio (Mac) or built-in iOS screen recording
  4. Upload to: YouTube Shorts (primary) + X + LinkedIn + LinkedIn India
  5. Embed in README + landing page

DELIVERABLE: Live URL · Public repo · Published video · A1-A40 QA report committed.

STOP CONDITIONS:
  - Smoke test on prod fails → ROLLBACK + debug + retry
  - Secret accidentally committed → BFG repo cleaner immediately + rotate key
  - Lighthouse on prod < 90 → fix before announcing

POST-DEPLOY:
  - Watch Worker logs for 30 min to confirm no errors
  - Set up Cloudflare alert on 5xx errors > 1% of requests
```

### 9.2 Acceptance gate
- [ ] clauk.app loads and is fully functional
- [ ] GitHub repo public with MIT license
- [ ] Demo video published on at least 2 channels
- [ ] No secrets in commit history
- [ ] A1-A40 QA report committed
- [ ] Production smoke test passes
- [ ] Worker logs clean for 30 min

### 9.3 Failure recovery
- Deploy fails: rollback via `wrangler pages rollback` (Cloudflare keeps last 50 deploys)
- Worker crashes: check logs, revert worker version
- Secret leaked: rotate key immediately + force-push history rewrite + notify

### 9.4 Time box: 7 hours

---

## Phase 10 — Launch (Week 2, Day 8-14, MIXED human + agent)

### 10.1 Goal
First 100 users, first 5 paying customers (Founder's Edition), Reddit/PH/HN/X presence.

### 10.2 Agent role
Draft posts in voice; human posts and responds (Reddit/HN bans bot accounts).

### 10.3 Daily schedule (per PRD v3 §13)

| Day | Channel | Agent task | Human task |
|---|---|---|---|
| D8 Mon | Reddit r/India, r/SideProject | Draft post | Post + respond to comments 4 hrs |
| D9 Tue | Reddit r/ClaudeAI, r/ChatGPTPro | Draft post | Post + respond |
| D10 Wed | Product Hunt | Draft maker comment + asset list | Submit at 12:01 AM PT |
| D11 Thu | Hacker News | Draft "Show HN" | Submit (only original maker can) |
| D12 Fri | X thread | Draft 8-tweet thread + demo video | Post + reply to mentions |
| D13 Sat | LinkedIn India | Draft long-form post | Post + tag relevant communities |
| D14 Sun | Catch-up | Compile feedback into V1.1 backlog | Decompress |

### 10.4 Agent prompt for each post
```
ROLE: Founder voice writer for Clauk launch.

GOAL: Write a [Reddit/PH/HN/X/LinkedIn] post for [Day X] launch.

CONSTRAINTS:
  - First-person ("I built") not third
  - Honest, not promotional
  - Include specific numbers: $720/yr alternative, ₹2,000/yr Clauk, 50/day free Gemini
  - Mention MIT open-source explicitly
  - End with explicit ask: feedback / install / star repo
  - No emoji unless on X
  - Length: Reddit 300-500 words · PH 250 words · HN 100 words · X 280 chars per tweet · LinkedIn 600-900

CONTEXT TO USE:
  - The Sand-West Karabiner F5 macro pain (Reddit r/ClaudeAI Dec 2024)
  - The Mar 2025 "Conversation Transfer Template" thread
  - The Manthan Patel LinkedIn 173-comment thread on AI switching frustration
  - The $720/yr alternative math
  - The lunch-money pricing reframe

DO NOT:
  - Bash competitors (mention them only factually)
  - Make claims you can't back up
  - Overpromise V1.1/V1.2 features as current

DELIVERABLE: Three variants per post (different opening hooks, same body) — human picks one.
```

### 10.5 Acceptance gate
- [ ] All 5 posts published on schedule
- [ ] PH product on front page (top 10) for at least 4 hours OR > 100 upvotes
- [ ] HN post on front page for at least 1 hour OR > 50 points
- [ ] Reddit posts: > 50 upvotes total across r/India + r/ClaudeAI
- [ ] First 5 Founder's Edition sales (when payment goes live Week 4)

---

## Phase 11 — Post-Launch Iteration (Ongoing)

### 11.1 Daily standup (5 min, agent-led)
```
ROLE: Daily standup agent.

INPUT: Production logs (Cloudflare Workers + Pages) from last 24 hrs.

OUTPUT: Append to standup.log:
  - Active users (24h): X
  - New cards saved: X
  - Gemini calls: X / 75,000 daily budget
  - Errors: X (top 3 by frequency)
  - Pending TODOs: X items in V1.1 backlog
  - Recommended priority for next session

STOP CONDITIONS:
  - Error rate > 5% of requests → page human immediately
  - Gemini quota > 80% of daily cap → enable Groq fallback in proxy
```

### 11.2 Weekly retrospective (30 min, human-led)
- Review week's feedback (Reddit, GitHub issues, email)
- Triage V1.1 backlog
- Decide what ships V1.1 vs deferred

### 11.3 V1.1 ship criteria (typically Week 3-4)
Ship V1.1 when ANY of:
- Top user complaint has technical fix < 2 hrs work
- New OS/browser version breaks something
- New free LLM provider available

---

## Anti-Patterns the Agent Must Auto-Reject

If user or agent suggests any of these, **immediately stop and escalate**:

| Suggestion | Why reject |
|---|---|
| Add React/Vue/Svelte | Violates D-3416 |
| Add cookies for analytics | Violates D-3410 |
| Switch to per-token billing | Violates D-3201 (audience) + D-3303 (bundle moat) |
| Add screen capture / OCR / accessibility-tree parsing | Violates D-2606, D-3002 |
| Add login bridge / cookie injection / session hijacking | Violates D-2905 |
| Sell anonymized user data | Removed from PRD v2; permanent rejection |
| Build native iOS/Android app for V1 | Violates D-2602 (V1-V2 only) |
| Add subscription on core features | Violates D-401 |
| Skip iOS install overlay | Violates D-2901 (existential for iOS retention) |
| Use AGPL/GPL/proprietary dep | Violates D-3414 |
| Add a framework "just for state management" | Violates D-3416 |
| Add ads | Permanently forbidden |
| Add browser-agent task automation | Comet's lane (D-3002) |
| Pivot pricing under pressure | Lunch-money pricing locked (D-3301) |

When any of these appear in a request, the agent must respond:
> "This conflicts with locked decision D-XXXX. I cannot proceed without explicit founder approval AND a written deviation note in decisions.log. Please confirm whether to escalate."

---

## Sanity Checks the Agent Must Run

### Before every commit
1. `grep -ri "API_KEY\|SECRET\|PASSWORD" public/` → must return 0 results
2. `grep -ri "console.log" public/` → review each; remove debug logs
3. License check on any new dep: `npm view <package> license` → must be MIT, Apache-2.0, BSD, or ISC

### Before every deploy
1. Build artifact size: `du -sh public/` → must be < 250 KB total
2. Lighthouse on preview URL → ≥ 90 on all axes
3. Smoke test list (from Day 7 Part B)
4. Worker request count today < 80% of free-tier budget

### Before every customer-facing change
1. Does it violate any of the 14 anti-patterns above?
2. Does it break any A1-A40 acceptance criterion?
3. Has the founder approved if it changes a D-decision?

---

## Escalation Matrix

| Situation | Severity | Escalate to | Within |
|---|---|---|---|
| Production down | P0 | Human, page immediately | 5 min |
| Secret leaked | P0 | Human + rotate keys | 10 min |
| Anti-pattern attempt | P1 | Human, written confirmation needed | 1 hr |
| Acceptance gate fails | P1 | Human review before proceeding | Same day |
| Lighthouse drops below 90 | P2 | Fix in next session | 24 hrs |
| Worker quota > 80% | P2 | Add provider fallback | 24 hrs |
| User feedback negative pattern | P3 | V1.1 backlog | Next sprint |

---

## File Layout the Agent Must Maintain

```
clauk-app/clauk/
├── README.md
├── LICENSE                    (MIT)
├── MASTER_CONTEXT.md          (ground truth for context)
├── clauk-prd-v3.md            (ground truth for scope)
├── AGENT_RUNBOOK.md           (this file)
├── ARCHITECTURE.md            (from Phase 2)
├── decisions.log              (every deviation from PRD v3, with rationale)
├── agent.log                  (every agent action, append-only)
├── validation-log.md          (from Phase 1)
├── standup.log                (from Phase 11)
├── public/
│   ├── index.html
│   ├── manifest.json
│   ├── sw.js
│   ├── privacy.html
│   ├── in/index.html
│   ├── icons/
│   ├── locales/
│   │   ├── en.json
│   │   ├── hi.json
│   │   ├── es.json
│   │   └── pt-BR.json
│   └── lib/                   (vendored MIT/Apache deps)
├── worker/
│   ├── llm-proxy.js
│   ├── wrangler.toml
│   └── README.md
└── .github/
    ├── workflows/
    │   └── deploy.yml         (V1.1)
    └── ISSUE_TEMPLATE/
        ├── bug.md
        └── feature.md
```

---

## Done Criteria — V1 Complete

The agent declares V1 done when **ALL** of these are true:

- [ ] clauk.app live + fully functional
- [ ] api.clauk.app worker live + multi-provider chain working
- [ ] GitHub repo public + MIT licensed
- [ ] All A1-A40 acceptance criteria PASS or documented WAIVER
- [ ] Lighthouse: Performance ≥ 95, A11y ≥ 95, Best Practices ≥ 95, SEO ≥ 90
- [ ] iOS install flow tested on real iPhone or iPad
- [ ] Android install flow tested on real Android device
- [ ] /privacy page covers GDPR + DPDPA + CCPA + LGPD
- [ ] /in landing page exists with Hindi + INR
- [ ] Multi-currency display works for at least 5 locales
- [ ] Demo video published on YouTube + X + LinkedIn
- [ ] At least one Reddit post live with > 20 upvotes
- [ ] Cloudflare daily quota usage < 30% of free tier
- [ ] No secrets in commit history
- [ ] decisions.log has < 5 deviations from PRD v3 (more = scope creep concern)

When all checked: human declares V1 complete + V1.1 sprint planning begins.

---

# APPENDIX A — Full Decision Log (D-101 through D-3422)

Every locked decision. Agent must cite the D-XXXX number when implementing.

## Foundational (D-101 to D-401)
| # | Decision |
|---|---|
| D-201 | Never silently mutate user input. Preserve raw + cleaned + final versions. |
| D-401 | Three promises: (1) free for core forever, (2) stays on device, (3) works in any AI. Public commitment. |

## Stack and platform (D-1101, D-2601-2602, D-3414-3420)
| # | Decision |
|---|---|
| D-1101 | i18n scaffold Day 1. `t()` function + `data-i18n` attr. en/hi/es/pt-BR locales. |
| D-2601 | `idb` (Jake Archibald MIT, 8 KB) for IndexedDB. Industry standard. |
| D-2602 | PWA over React Native or Tauri for V1. Single codebase, no app-store cut. |
| D-3414 | 100% MIT or Apache 2.0 deps. No AGPL, no GPL, no proprietary. |
| D-3415 | No build system V1. Single HTML, inline JS/CSS. |
| D-3416 | No framework V1. Vanilla JS. |
| D-3417 | No backend except free LLM proxy. Frontend-first. |
| D-3418 | Free LLM proxy = Cloudflare Worker chain (Gemini → Groq → OpenRouter). |
| D-3419 | Open-source from Day 7. GitHub public, MIT. |
| D-3420 | Self-hostable as a feature. "Don't trust us? Run it yourself." |

## Export and capture (D-2003, D-2207)
| # | Decision |
|---|---|
| D-2003 | Export defaults: `cm.md` (Clauk-native) + `AGENTS.md` (Linux Foundation universal). |
| D-2207 | 6 capture surfaces sequenced over 12 months. V1 ships #1-4 only. |

## iOS hardening (D-2901-2903)
| # | Decision |
|---|---|
| D-2901 | iOS install overlay. Detect iPhone/iPad UA + standalone state. Show after 2nd card save. |
| D-2902 | Dual-write last 50 cards to localStorage on iOS. Defends against IndexedDB eviction. |
| D-2903 | "Open weekly" educational toast on iOS first save. Soft education on 7-day eviction. |

## Permanent refusals (D-2606, D-2905, D-3002)
| # | Decision |
|---|---|
| D-2606 | No screen capture. No passive surveillance. Ever. |
| D-2905 | No login bridge / cookie injection / session hijacking. Ever. |
| D-3002 | Do not compete with Comet (browser agent) or Rewind/Limitless (passive capture). Refusing those lanes IS the moat. |

## Five legitimate bridges (D-2906)
1. Deep links — claude.ai, chat.openai.com, gemini.google.com prefill where supported (V1)
2. Web Share Target — receive shares from any app (V1 Day 6)
3. Browser extension — Manifest V3 with `activeTab` only (V1.2)
4. BYOK API — user provides own keys (V2, partial in V1)
5. Free LLM passthrough — Cloudflare Worker proxy (V1 Day 6 backend)

## Core idea + audience (D-3001, D-3003, D-3201-3206)
| # | Decision |
|---|---|
| D-3001 | Core idea: "The structured memory layer for people who'd rather not have an AI watching them." Slogan: "Save your AI context once. Carry it to any AI. Without the surveillance tax." |
| D-3003 | Smart auto-categorization on paste = regex/heuristics only. No AI inference. |
| D-3201 | Primary audience: students + builders who can't afford 3 paid AIs. |
| D-3202 | Free Gemini passthrough ships V1 Day 6, not V2. Memory layer is the gift inside the AI-access wrapper. |
| D-3203 | No payment integration in V1. Defer monetization to V1.2 Week 4. |
| D-3204 | Rate limiting Day 1: per-fingerprint 50/day Gemini, 500/day Groq. |
| D-3205 | Per-user fingerprint via SubtleCrypto SHA-256 hash of UA + screen + timezone. Local only. |
| D-3206 | University outreach plan for Year 2. Target 10 universities for verified `.edu` priority quota. |

## Pricing and monetization (D-3301-3304, D-3401-3405)
| # | Decision |
|---|---|
| D-3301 | Lunch-money pricing locked: ₹2,000 / £19 / $24 / €22 yearly · ₹2,500 / £23 / $29 / €27 lifetime (500-cap). |
| D-3302 | Three-layer daily-return monopoly: L1 mechanical (quota reset) · L2 re-engagement (Card of Day + Open Threads) · L3 habit (streak + reflection). |
| D-3303 | Monopoly is Audience + Trust + Bundle, not tech. |
| D-3304 | Migrate-friendly = trust moat. Tagline: "Outgrowing Clauk? Click Export. Drop the cm.md into any other tool." |
| D-3401 | India primary market. INR pricing. Razorpay primary. |
| D-3402 | UK + USA secondary. GBP/USD. Stripe Payment Links. |
| D-3403 | EU + Brazil + Indonesia + Vietnam + Philippines tertiary. No paid tier V1; revisit Year 2. |
| D-3404 | i18n stubs V1: en (complete) · hi · es · pt-BR. Full translation V1.1. |
| D-3405 | Pricing display via `navigator.language` detection. Manual override in Settings. |

## Compliance posture (D-3406-3413)
| # | Decision |
|---|---|
| D-3406 | Local-first means most compliance is automatic. Zero data → most laws moot. |
| D-3407 | GDPR section in /privacy. Cookie banner not required (zero cookies). |
| D-3408 | DPDPA India section in /privacy. Zero collection makes provisions inapplicable. |
| D-3409 | CCPA California section in /privacy. No data sale (no data to sell). |
| D-3410 | Cookie-free architecture. localStorage + IndexedDB only. |
| D-3411 | Payment data handled exclusively by Stripe/Razorpay. Clauk never sees card numbers. |
| D-3412 | Free LLM passthrough requires per-call disclosure. "Your question goes to Google AI Studio; their privacy policy applies." |
| D-3413 | No telemetry/analytics V1. Opt-in only V1.2+. |

## Frontend-first scope (D-3421-3422)
| # | Decision |
|---|---|
| D-3421 | Frontend-first scope locked V1. Everything client-side except free LLM proxy. |
| D-3422 | Stripe Payment Links + Razorpay Payment Pages for V1.2 monetization. No payment backend. Webhooks only if conversion proves material. |

## Steal-and-refuse master list (D-3107)
**STEAL:**
- MemPalace `/vs/X` comparison-page playbook
- brain-mcp cognitive-state UX (status + confidence fields)
- MemVid "single portable file" framing
- Plurality AI Context Flow buckets UI (scope field as "Buckets")
- myNeutron lifetime-deal financing
- Supermemory startup-credit program (Year 2)
- Mem0 industry-report citations in launch posts
- AGENTS.md Linux Foundation universal standard

**REFUSE:**
- Per-token billing (Supermemory)
- Cloud-first architecture (Plurality, Mem0)
- Screen capture (Rewind, Mirix)
- Browser-agent lane (Comet)
- VC funding pressure (Rewind→Meta cautionary tale)
- Medical metaphors (regulatory + brand collision)
- App Store V1 (30% cut + review delays)
- Account requirement

---

# APPENDIX B — Acceptance Criteria A1-A40

Agent must verify each before declaring V1 done. PASS / FAIL / WAIVER with reason.

| # | Criteria | Test |
|---|---|---|
| A1 | Vault encrypts cards at rest | DevTools → IndexedDB → cards store shows ciphertext, not plaintext |
| A2 | Vault decrypts on read | Save card → reload → view card → content visible |
| A3 | Multi-format export works for all 6 formats | Export each → file downloads with correct extension + content |
| A4 | Wipe all data requires typed DELETE | Type partial → button disabled · type DELETE → button enabled |
| A5 | Privacy Verifier shows 0 fetches in capture flow | Open Verifier → save card → counter stays at 0 |
| A6 | Privacy Verifier shows persistent storage status | Settings → Privacy → row shows ✅ or ⚠️ |
| A7 | i18n switches language without reload | Settings → Language → pick hi → UI updates immediately |
| A8 | Starter pack installs 10 example cards | Tap "Install 10 examples" → list shows 10 cards |
| A9 | Paste from clipboard works | Copy text → tap Paste → textarea fills |
| A10 | Auto-categorization fills type/source/title/tags | Paste Decision-type content → type chip = Decision active |
| A11 | Continue conversation mode wraps prompt | Toggle Continue → tap Open in Claude → clipboard contains continuation framing |
| A12 | Ask Gemini returns response < 5 sec | Tap Ask Gemini → response appears in panel < 5 sec |
| A13 | Free quota decrements on each call | Counter 50→49 after first call |
| A14 | Rate limit 429 after 50 calls | Spam 51 calls → 51st returns 429 with retryAfter |
| A15 | BYOK Gemini routes to user's key | Add BYOK key → call Ask Gemini → user's quota used |
| A16 | Deep link Claude opens with prompt | Tap Open in Claude → claude.ai opens with q= param |
| A17 | Web Share Target receives shares | Android: share text from another app → Clauk opens with content |
| A18 | iOS install overlay shows on iOS after 2nd save | Test on real iPhone, save 2 cards → overlay appears |
| A19 | iOS dual-write to localStorage | Save 5 cards on iOS → DevTools → localStorage has backup |
| A20 | navigator.storage.persist() requested on first save | Save first card → permission prompt or auto-granted |
| A21 | Card list search filters in real time | Type query → list updates as keystrokes happen |
| A22 | Sort dropdown works | Switch sort → list reorders |
| A23 | Filter chips work | Tap Decision chip → list shows only Decisions |
| A24 | Open Threads chip count accurate | Count = pending Decisions + hypothesis Discoveries + stale Projects |
| A25 | Card of the Day surfaces a card | Open home → "Still relevant?" card visible |
| A26 | Streak counter increments on consecutive days | Save Day 1 → save Day 2 → streak = 2 |
| A27 | Streak resets on missed day | Save Day 1 → skip Day 2 → save Day 3 → streak = 1 |
| A28 | "Open weekly" iOS toast fires once | First save on iOS shows toast; subsequent saves don't |
| A29 | Founder's Edition $29 Stripe Payment Link works | Tap "Get notified" → opens Stripe Checkout (placeholder OK V1) |
| A30 | Sync waitlist email captures to localStorage | Enter email → toast confirm → DevTools → localStorage `clauk_pricing_waitlist` has entry |
| A31 | Continue mode produces correct prompt wrapping | Verify clipboard contents match continuation template |
| A32 | Currency display matches locale | Set `navigator.language='hi-IN'` → ₹2,000 displayed |
| A33 | `/privacy` covers GDPR + DPDPA + CCPA + LGPD | All 4 sections present, ≤200 words each |
| A34 | `/in` landing has Hindi tagline + INR | Visit /in → Hindi heading + ₹2,000 visible |
| A35 | Cookie-free verified | DevTools → Application → Cookies → 0 entries |
| A36 | Card of the Day surfaces random old card | Different seed → different card surfaced |
| A37 | Open Threads chip filters correctly | Tap chip → list shows only open-thread cards |
| A38 | Streak counter persists across reloads | Save card → reload → streak preserved |
| A39 | Free Gemini quota UI displays 50/50 starting count | Settings → AI Access → "Free Gemini: 50/50 today" on fresh state |
| A40 | Continue conversation mode wraps prompt with source attribution | Verify wrap text exactly matches template |

---

# APPENDIX C — Glossary (24 terms)

| Term | Definition |
|---|---|
| Clauk | Product name. Domain: clauk.app. |
| Smart Harvest | Original (May 2 PRD) capture concept via accessibility-tree + OCR. Superseded by paste + Web Share Target. Historical only. |
| Context card | Atomic unit of vault. One of 10 typed cards. |
| `cm.md` | Clauk-native Markdown export with YAML frontmatter. Canonical portable file. |
| `AGENTS.md` | Linux Foundation universal AI-agent context format. Co-export with cm.md. |
| Switzerland positioning | "Neutral memory layer across walled-garden AI apps." Investor deck framing. |
| Lunch-money pricing | $24/yr · £19/yr · ₹2,000/yr · $29 lifetime. Impulse-purchase band. |
| Founder's Edition | 500-spot lifetime tier. Closes permanently after 500 sold. |
| Open Threads | Filter chip showing Decisions with status=pending + Discoveries with confidence=hypothesis. |
| Card of the Day | Daily re-engagement: random old card with "still relevant?" prompt. |
| Daily-return monopoly | 3-layer retention design (mechanical / re-engagement / habit). |
| Migrate-friendly | Marketing principle: leaving Clauk is one click. Trust through portability. |
| Five legitimate bridges | D-2906: deep links · Web Share Target · browser extension · BYOK API · free LLM passthrough. |
| Three promises | D-401: free for core forever · stays on device · works in any AI. |
| 7-check protocol | UI-skill quality gate: a11y, style-match, boolean-props, tokens, motion, mobile-first, reduced-motion+dark-mode. |
| 10-priority framework | UI-skill conflict resolver: lower # wins (a11y > touch > perf > style). |
| Vit | Founder pseudonym. |
| Path A / Path B | Strategic forks: A = independent/lifestyle · B = make it investable. Path A locked. |
| PWA | Progressive Web App. Clauk's distribution vehicle. |
| D-XXXX | Decision identifier convention. ~102 total currently. |
| QW1-QW12 | Day 6 quick wins. |
| A1-A40 | Acceptance criteria. |
| BYOK | Bring Your Own Key — user supplies own API key. |
| Sand-West | Reddit user (r/ClaudeAI Dec 2024) whose Karabiner F5 macro for chat summarization is cited as smoking-gun pre-validation. |

---

# APPENDIX D — Canonical Card Schema

```javascript
{
  id: "uuid-v4",
  type: "Bio" | "Role" | "Project" | "Goal" | "Decision" | "Discovery" | "Preference" | "Style" | "Person" | "Snippet",
  title: "string (max 120 chars)",
  content: "encrypted markdown string",
  tags: ["string"],
  scope: "personal" | "work" | "study" | "general",
  source: "claude" | "chatgpt" | "gemini" | "perplexity" | "grok" | "lechat" | "manual" | "imported",
  status?: "pending" | "resolved" | "abandoned",        // Decision cards only
  confidence?: "hypothesis" | "validated" | "invalidated", // Discovery cards only
  ts_created: 1234567890,                                // ms epoch
  ts_updated: 1234567890,
  ts_last_used: 1234567890,
  use_count: 0,
  schemaVersion: 1                                        // for V2 migration
}
```

**IndexedDB stores:**
- `cards` — keyPath `id`, indices on `type`, `scope`, `ts_created`, `ts_last_used`
- `meta` — keyPath `key`. Stores: `device_key` (encrypted AES-GCM key), `fingerprint`, `streak`, `lastCaptureDate`, `cardOfDayId`, `quotaUsed`, BYOK keys

**localStorage keys:**
- `clauk_settings` — locale, theme, currency override
- `clauk_pricing_waitlist` — array of {email, ts}
- `clauk_last_50` — iOS-only backup of 50 most recent cards
- `clauk_streak` — duplicated from IndexedDB for fast read
- `clauk_currency` — manual currency override

---

# APPENDIX E — Multi-Region Pricing Table

| Tier | India INR | UK GBP | USA USD | EU EUR | Brazil BRL | What it unlocks |
|---|---|---|---|---|---|---|
| **Free Forever** | ₹0 | £0 | $0 | €0 | R$0 | All capture · all export · 10 types · 50/day Gemini · 500/day Groq · unlimited cards on device |
| **Clauk One Year** | ₹2,000 | £19 | $24 | €22 | R$120 | + 200/day priority Gemini · 1,500/day priority Groq · all future paid features for 12 mo |
| **Founder's Edition** (500 cap) | ₹2,500 | £23 | $29 | €27 | R$150 | Lifetime everything · name in credits · direct line to founder |

**Payment processor by region:**
| Region | Primary | Fallback |
|---|---|---|
| India | Razorpay Payment Pages (UPI + cards) | Stripe |
| UK / EU | Stripe Payment Links | Lemon Squeezy (auto-EU-VAT) |
| USA | Stripe Payment Links | — |
| Brazil | Stripe Payment Links | — |
| Other | Stripe Payment Links | — |

**Free LLM API budget (per provider, daily, Clauk-aggregate):**
| Provider | Free tier daily limit | Per-user cap (Clauk) | Notes |
|---|---|---|---|
| Gemini AI Studio | 1,500 req | 50/day | Primary |
| Groq Llama 3.3 70B | 14,400 req | 500/day | Most generous; primary fallback |
| OpenRouter `:free` | rate-limited | 100/day | Tertiary fallback |
| Cloudflare Workers AI | free, Llama/Mistral | unused V1 | Quaternary fallback |

---

# APPENDIX F — Risk Register (8 active risks)

| Risk | Probability | Impact | Mitigation |
|---|---|---|---|
| Free Gemini quota abuse | High | Service breakage | Per-fingerprint cap (50/day) + per-IP cap (100/hr) + multi-provider fallback |
| Native LLMs ship full memory parity in 6 mo | Medium | Reduced wedge | Multi-region pricing + bundled free Gemini maintains differentiation |
| MemPalace pivots to mobile | Low | Direct competitor | Ship faster, own student lane |
| iOS PWA storage eviction | Medium | User data loss | D-2901-2903: install overlay + dual-write + weekly toast |
| Razorpay KYC delay (3-7 days India) | Medium | India payment block | Launch Stripe-only, add Razorpay Month 2 |
| EU compliance scrutiny | Low | Forced re-architecture | Zero-data architecture pre-compliant; /privacy in place |
| Solo founder burnout | High | Stagnation | Sustainable indie pace, accept smaller scope, no VC pressure |
| Cloudflare cost overrun | Low | $5 → $50/mo | Hard rate limits + manual review at 1K daily users |

---

# APPENDIX G — Open Questions (14 unresolved)

**Architectural:**
1. Free LLM proxy on Cloudflare Workers (current) or fallback to Deno Deploy / Fly.io?
2. License verification — signed JWT (offline-verifiable) or lookup endpoint (revocable)?
3. ZIP import (V1.2) client-side or via Cloudflare Worker?

**Operational:**
4. Domain: clauk.app / clauk.dev / clauk.io?
5. GitHub org: clauk-app / vit-clauk / personal?
6. Hindi translation: DIY / hire / machine + native review?
7. Razorpay KYC: start this week or post Week 1?

**Product:**
8. Founder's Edition cap: 500 (locked) vs 1,000?
9. Include `/vs/X` pages in V1 or defer to Week 2?
10. Demo video: horizontal master + repurpose, or vertical-only?
11. Gemini quota reset: midnight UTC (simple) or per-user-local-midnight (fair)?

**Strategic:**
12. When to apply for Anthropic/OpenAI/Google academic grants?
13. Publish monthly revenue publicly (transparency play) or stay private?
14. When to open second product line (Teams) vs deepen Clauk?

Agent flags these in `decisions.log` when encountered. Human resolves.

---

# APPENDIX H — Launch Post Drafts (Week 2)

Pre-drafted by agent. Human posts manually (Reddit/HN ban bot accounts).

## D8 Mon — Reddit r/India + r/SideProject

> **Title:** "Built free AI memory for Indian students. ₹2,000/yr or free forever. Open source."
>
> Hey r/India. I'm Vit, an indie founder. I just shipped Clauk — a privacy-first AI memory app built for students who can't afford $720/yr for ChatGPT + Claude + Gemini.
>
> The problem: every AI tool has its own memory. You switch tools, you re-explain yourself. A CS student in Bangalore using all three free tiers hits limits constantly and wastes hours rebuilding context.
>
> What Clauk does: save your context once on your phone (encrypted, on-device), reuse it in any AI tool via copy-paste, export, or our free Gemini passthrough (50 calls/day, no signup, no cost).
>
> Pricing: free forever for everything that matters. ₹2,000/year unlocks priority quota. ₹2,500 lifetime (500 spots only).
>
> Stack: MIT-licensed PWA. No accounts. No cloud. No ads. No surveillance. Runs in your browser.
>
> Live at clauk.app. GitHub: github.com/clauk-app/clauk (fork it, self-host it).
>
> Honest ask: install it, break it, tell me what's wrong. AMA.

## D9 Tue — Reddit r/ClaudeAI + r/ChatGPTPro

> **Title:** "I solved my own multi-AI context problem — a free local-first memory app"
>
> Last December, /u/Sand-West posted a Karabiner F5 macro that sends a "summarize this conversation" prompt to Claude so they could continue elsewhere. In March 2025, /u/[transfer-template-author] posted a 7-section manual template for the same reason.
>
> I built the app version of that workflow. Clauk is a PWA: paste your conversation, it auto-categorizes (Decision / Discovery / Project / Bio / etc.), encrypts on-device, exports to cm.md / AGENTS.md / CLAUDE.md / GEMINI.md / .cursorrules / JSON.
>
> Continue conversation mode wraps your context as "Here's a conversation I was having with Claude. Continue from where it left off. My next question: ___" — one tap replaces your whole Karabiner workflow.
>
> Free forever for capture/export. $29 lifetime if you want to support indie dev.
>
> No accounts. No cloud. MIT licensed. clauk.app.

## D10 Wed — Product Hunt

> **Tagline:** "Free AI memory + free Gemini for students"
>
> **Description:** Clauk solves the multi-AI context problem with a privacy-first, mobile-first PWA. Save context once, reuse anywhere. Includes free Gemini access (50/day) and free Groq (500/day) so students don't need $720/yr for AI subscriptions.
>
> **What makes it different:** local-first (no cloud), MIT open-source (fork it), multi-region pricing (₹2,000 / £19 / $24), zero telemetry, zero cookies. Migrate-friendly: export to cm.md or AGENTS.md anytime.

## D11 Thu — Hacker News

> **Show HN:** Clauk — local-first AI memory PWA. Capture free forever, optional $24/yr.
>
> Solo founder. 7-day build. MIT licensed. No accounts, no cloud, no tracking. Vanilla JS in a single HTML file. Cloudflare Workers proxy for free Gemini/Groq passthrough. Multi-region pricing for students in India + UK + USA.
>
> clauk.app · github.com/clauk-app/clauk

## D12 Fri — X (8-tweet thread)

> 1/ I just shipped Clauk. Free AI memory + free Gemini access for students who can't afford $720/yr for ChatGPT + Claude + Gemini. 🧵
>
> 2/ The problem: students juggle 3 free-tier AIs because each hits limits. They re-explain themselves constantly. A CS student in Bangalore loses ~5 hours/week to context-switching.
>
> 3/ Clauk saves your context once, encrypted, on your phone. Then reuses it in any AI via deep link, copy-paste, or our free Gemini passthrough (50 calls/day).
>
> 4/ Pricing: free forever for everything that matters. ₹2,000/yr or $24/yr unlocks priority quota. $29 lifetime if you want to support indie dev. Founder's Edition capped at 500.
>
> 5/ Why I built it: I'm a PM, I use 4 AI tools daily, I hate copy-pasting. So I built the tool I wanted. Solo founder. 7 days. MIT open-source.
>
> 6/ What it isn't: not a Comet competitor (browser agent), not Rewind (screen capture), not Mem0 (cloud SaaS). Refusing those lanes IS the moat.
>
> 7/ Stack: PWA, vanilla JS, Cloudflare Pages + Workers, idb for IndexedDB, AES-GCM 256 on-device encryption. ~$5/mo to run at scale.
>
> 8/ Live at clauk.app. GitHub at github.com/clauk-app/clauk. Star if you'd use it. Fork if you want to self-host. Tell me what's broken.

## D13 Sat — LinkedIn India

> **Headline:** How Indian students can save ₹60,000/year on AI tools
>
> [Open with the cost-comparison table: ChatGPT ₹19,990/yr + Claude ₹19,990/yr + Gemini ₹19,490/yr = ₹59,470 for the full stack.]
>
> Most CS students I talk to in Bangalore, Pune, and Hyderabad use all three on free tiers. They hit limits constantly. They re-explain context every session. They lose hours of work.
>
> I built Clauk because I had the same problem. It's a free PWA that saves your context once and reuses it anywhere — including free Gemini and Groq access bundled in. ₹2,000/year unlocks priority. Or use it free forever.
>
> No accounts. No cloud. Open source (MIT). Built for students who can't afford the alternatives.
>
> Try it: clauk.app

---

# APPENDIX I — V1.1 / V1.2 / V2 Backlog

## V1.1 (Week 2-3, ~20 hours)
- Streak counter UI + logic with daily check
- Daily reflection prompt ("What did you learn today?")
- Smart auto-categorization regex tuning based on real-user data
- Continue conversation prompt wrapping refinements
- Open Threads chip wired to real schema (status + confidence fields surface in card detail)
- Hindi full translation (replace stub)
- 4 comparison pages: /vs/mempalace, /vs/supermemory, /vs/ai-context-flow, /vs/mem0
- Browser extension preparation (manifest, content-script skeleton, deferred)

## V1.2 (Month 1-2, ~80 hours)
- Browser extension shipped (Chromium Manifest V3, activeTab only)
- ZIP import for ChatGPT / Claude / Gemini export bundles
- Stripe Payment Links active (was placeholder)
- Razorpay Payment Pages active (post-KYC)
- Lemon Squeezy fallback for EU
- License verification (signed JWT or lookup endpoint — open question)
- Sync waitlist email export pipeline (manual CSV → Mailchimp / Buttondown)
- AppSumo submission for Founder's Edition

## V2 (Month 6+, ~200 hours)
- Tauri desktop wrapper with clipboard watcher
- Cloud sync ($3/mo optional, E2E encrypted, BYOK)
- "Ask AI" tab — brain-mcp-inspired retrieval over user vault
- University partnerships (verified `.edu` priority)
- Foundation grant applications (Anthropic, OpenAI, Mozilla, Linux Foundation)
- Teams tier ($12/seat) for hackathons + startup teams
- Enterprise edition (self-hosted, paid)

## Year 2 sustainability options
- Native iOS + Android apps if PWA retention hits ceiling
- Optional cloud LLM tier (BYOK to user's own paid Claude/GPT key)
- Foundation funding (grants only — no equity)

## Never on roadmap
- Subscription on core features (violates D-401)
- Ads (permanent)
- Data sale / licensing (permanent)
- Screen capture (D-2606)
- Browser-agent automation (D-3002)
- Login bridge (D-2905)

---

# APPENDIX J — Six Capture Surfaces Sequence (D-2207)

| # | Surface | Ships in | How |
|---|---|---|---|
| 1 | Paste from clipboard | V1 Day 4 | `navigator.clipboard.readText()` + permission prompt |
| 2 | Web Share Target (Android system share sheet) | V1 Day 6 | Manifest `share_target` + `/?share=1` URL handler |
| 3 | Manual type/edit | V1 Day 4 | Textarea in capture sheet |
| 4 | Starter pack install (10 examples) | V1 Day 3 | Bundled JSON, decrypted + saved to vault |
| 5 | Browser extension (Manifest V3, activeTab) | V1.2 Month 2 | Right-click selection → "Save to Clauk" |
| 6 | ZIP import (ChatGPT/Claude/Gemini export bundles) | V1.2 Month 2 | Client-side ZIP parse + cm.md mapping |

**Permanently excluded:**
- Accessibility-tree parsing for live AI apps (fragile, Android-only)
- Screen capture / OCR (D-2606)
- Login bridge / session injection (D-2905)
- Browser-agent capture (D-3002)

---

# APPENDIX K — Three Personas (For Marketing Voice)

## Aryan — primary
- Computer Science student, Bangalore, 21, BTech 3rd year
- Side projects + interview prep + 2 internships
- Uses ChatGPT free + Claude free + Gemini free in parallel
- Discretionary spend: ₹500-2,000/month
- **Willing to pay ₹2,000/year ($24)**
- Mid-range Android (Realme/Redmi), often on 4G
- Privacy-aware but not paranoid
- Shares tools via WhatsApp + Discord

## Maya — secondary
- PhD researcher, Manchester, 26
- Social sciences, hates Notion/Mendeley overhead
- Claude for lit review · ChatGPT for drafting
- Subscription budget: £10-30/month max
- **Willing to pay £19/year**
- iPhone, often on shared university Wi-Fi
- GDPR-aware, reads privacy policies

## Marco — tertiary
- Indie hacker, Austin, 32
- Building 3rd SaaS attempt while consulting
- Claude Code daily + GPT-4 marketing + Perplexity research
- Pays $60/mo across AI tools, wants to consolidate
- **Willing to pay $29 lifetime / $24/year**
- Mac + iPhone, technical, expects open-source

---

# APPENDIX L — Compliance Map by Jurisdiction

| Jurisdiction | Law | Clauk posture | `/privacy` section |
|---|---|---|---|
| EU + UK | GDPR + UK-GDPR | Zero data collected. Right-to-delete = built-in Wipe. No cookie banner needed. | List of data we don't collect + Wipe button reference |
| India | DPDPA 2023 | Zero collection. Cross-border transfer rules N/A (no transfer). | Explicit nil-collection statement + free LLM passthrough notice |
| California USA | CCPA + CPRA | No data sale (no data). | Right-to-know + right-to-delete (both automatic) |
| Brazil | LGPD | Zero collection. | Minimal — refer to GDPR section |
| Other | Various | Same posture | Generic "we don't collect" + jurisdiction-aware Wipe |

**Critical compliance insight:** Local-first architecture is the strongest possible compliance posture. The `/privacy` page is mostly a list of what we don't do, plus the free LLM passthrough disclosure (Gemini/Groq do receive queries — their policies apply).

---

# APPENDIX M — Revenue Forecast (Year 1, Conservative)

| Month | Free users | One Year sold | Lifetime sold | Cumulative revenue |
|---|---|---|---|---|
| M1 | 500 | 30 | 20 | ~$1,300 |
| M2 | 1,500 | 60 | 25 | ~$2,500 |
| M3 | 3,500 | 100 | 30 | ~$4,000 |
| M6 | 12,000 | 350 | 100 | ~$10,000 |
| M9 | 25,000 | 600 | 200 | ~$17,500 |
| M12 | 50,000 | 1,200 | 500 (closed) | **~$30,000** |

Multi-region pricing makes target achievable. India = volume. UK/USA = ARPU. Combined = $30K Year 1 with solo founder + $5/mo burn = indie-sustainable.

---

# APPENDIX N — Tech Stack Bill of Materials

| Layer | Choice | License | Bundle size | Why |
|---|---|---|---|---|
| Hosting | Cloudflare Pages | — | 0 | Free, global edge |
| Markup/Style/Logic | HTML5 + CSS3 + ES2022 | — | 0 | No framework |
| IndexedDB wrapper | `idb` 8.x | MIT | 8 KB | Jake Archibald, industry standard |
| Encryption | Web Crypto API | Native | 0 | Browser-built-in |
| PWA shell | Manifest + SW | Native | 0 | Browser-built-in |
| Share intake | Web Share Target API | Native | 0 | Android OS integration |
| Persistence | `navigator.storage.persist()` | Native | 0 | Browser-built-in |
| i18n | Custom t() + JSON | Our code | 2 KB | Lightweight |
| Search | `Fuse.js` 7.x | Apache 2.0 | 14 KB | Loaded only when card count > 100 |
| Markdown render | `marked.js` 12.x | MIT | 30 KB | Preview only |
| LLM proxy | Cloudflare Worker | — | 0 | $5/mo, free tier sustains 1K users |
| Payments | Stripe + Razorpay Payment Links | — | 0 | No-code, hosted checkout |
| Analytics | None V1 | — | 0 | D-3413 |
| Desktop (V2) | Tauri 2.x | MIT/Apache 2.0 | 3 MB native | Smaller than Electron |
| Browser ext (V1.2) | Manifest V3 + activeTab | — | <50 KB | Privacy-preserving |

**Total V1 bundle target:** <80 KB gzipped (currently ~135 KB raw / ~35 KB gzipped — within target).
**Zero AGPL. Zero GPL. Zero proprietary.**

---

# APPENDIX O — Founder's Cost Comparison (For Pitch Anchor)

What students/builders currently pay for the full AI stack:

| Region | ChatGPT Plus | Claude Pro | Gemini Advanced | Total/yr |
|---|---|---|---|---|
| USA | $240 | $240 | $240 | **$720** |
| UK | £192 | £192 | £192 | **£576** |
| India | ₹19,990 | ₹19,990 | ₹19,490 | **₹59,470 (~$715)** |
| EU | €228 | €228 | €228 | **€684** |
| Brazil | R$1,176 | R$1,176 | R$1,176 | **R$3,528** |

Plus optional memory tools (Mem0 / Supermemory / Plurality / AI Context Flow): **+$60-228/yr**.

**Grand total for full multi-AI + memory stack:**
- USA: $780-948
- UK: £624-756
- India: ₹64,000-78,000

**Clauk's offer:** ₹2,000 / £19 / $24 per year. **~1/40th the cost** of the maximalist alternative.

For an Indian CS student earning a ₹15,000/month stipend: ₹64,000/year for AI tools = ~6 months of full discretionary budget. Unaffordable. ₹2,000 = ~1 week of lunch. Achievable.

---

**End of AGENT_RUNBOOK.md. Self-contained. Any agentic AI encountering ambiguity must stop and ask, never guess.**
