# Buen Chico — Master Plan

**Spanish-first bilingual AI dog training app.**

> "The bilingual gap in dog training apps is wide open. Every competitor is
> English-first with bolted-on translations. We go Spanish-first with
> English as the second language — and use AI where competitors use static content."

---

## Table of Contents

1. [Product Vision](#1-product-vision)
2. [Competitive Landscape](#2-competitive-landscape)
3. [Research Roadmap](#3-research-roadmap)
4. [Training Data Strategy](#4-training-data-strategy)
5. [Architecture & Tech Stack](#5-architecture--tech-stack)
6. [Tools We Already Have](#6-tools-we-already-have)
7. [What We Need to Build / Buy](#7-what-we-need-to-build--buy)
8. [MVP Scope](#8-mvp-scope)
9. [Phase Plan](#9-phase-plan)
10. [Cost Estimates](#10-cost-estimates)
11. [Open Questions](#11-open-questions)

---

## 1. Product Vision

**Buen Chico** is a mobile app that teaches dog owners how to train their dogs — in Spanish first, English second. It combines:

- **AI-generated personalized training plans** based on breed, age, behavior problems, and owner experience level
- **Short-form video lessons** (15-60s) with bilingual voiceover and captions
- **Progress tracking** with photo/video check-ins
- **AI chat trainer** — ask questions in Spanish or English, get real answers grounded in certified training methodology (positive reinforcement / force-free)

**Why Spanish-first wins:**
- 500M+ Spanish speakers worldwide, 42M in the US alone
- Every competitor (Dogo, Puppr, GoodPup, Traini) is English-first
- The "well-trained" dog training content in Spanish is almost nonexistent in app form
- Hispanic pet ownership in the US is the fastest-growing segment (APPA 2024)
- Cultural angle: multigenerational households where abuela speaks Spanish but the teen speaks English — app works for both

---

## 2. Competitive Landscape

### Direct Competitors (Dog Training Apps)

| App | Revenue | Model | Language | AI? | Weakness |
|-----|---------|-------|----------|-----|----------|
| **Dogo** | ~$10M ARR | Subscription $9.99/mo | English-first, 8 languages | Basic (clicker timing) | Generic plans, no real personalization, Spanish is Google Translate quality |
| **Puppr** | ~$3M ARR | Subscription $9.99/mo | English only | None | Static lesson library, no video, no Spanish at all |
| **GoodPup** | ~$5M ARR | Live trainer $29.99/wk | English only | None (human trainers) | Expensive, doesn't scale, English-only |
| **Traini** | ~$2M ARR | Subscription $7.99/mo | English-first | Yes (most dangerous) | AI-generated plans but English-only, small team |
| **Barkly** | <$1M | Freemium | English only | Basic | Minimal features |

### Indirect Competitors
- YouTube dog training channels (Zak George, McCann Dogs) — free but unstructured
- TikTok trainers — entertainment, not curriculum
- Local trainers ($50-150/session) — expensive, inconsistent quality
- Books/PDFs — not interactive

### Our Edge
1. **Spanish-first** — nobody owns this. Period.
2. **AI personalization** — not just "here's a generic sit lesson" but "your 8-month GSD is pulling on leash AND resource guarding — here's your prioritized plan"
3. **Video-first with AI voiceover** — we can generate unlimited bilingual content using our existing pipeline (fal.ai + ElevenLabs + content-loop)
4. **Price**: target $4.99/mo (undercut Dogo/Puppr by 50%)

---

## 3. Research Roadmap

### Phase A: Training Methodology Research (BEFORE building anything)

| # | Research Task | Source | Purpose | Status |
|---|--------------|--------|---------|--------|
| A1 | Certified positive-reinforcement training curricula | AKC S.T.A.R. Puppy, CPDT-KA study materials, Karen Pryor Academy, Ian Dunbar books | Ground truth for lesson content — we do NOT make up training advice | ⬜ TODO |
| A2 | Spanish-language dog training content audit | YouTube ES channels, libros de adiestramiento canino, RSCE (Spain), Mexican kennel clubs | What exists? What's the quality? Where are the gaps? | ⬜ TODO |
| A3 | Breed-specific behavior profiles | AKC breed database, UKC, FCI (Fédération Cynologique Internationale — covers Latin America) | Map 200+ breeds to temperament, common problems, training approach | ⬜ TODO |
| A4 | Common behavior problems taxonomy | AVSAB position statements, veterinary behaviorist literature | Structured problem classification: aggression, anxiety, reactivity, house training, etc. | ⬜ TODO |
| A5 | Force-free / positive reinforcement evidence base | Journal of Veterinary Behavior, Applied Animal Behaviour Science | Scientific backing for our methodology — also marketing differentiation | ⬜ TODO |
| A6 | Hispanic pet ownership demographics | APPA National Pet Owners Survey, Census data, Packaged Facts reports | Market sizing, user persona development | ⬜ TODO |

### Phase B: Technical Research

| # | Research Task | Source | Purpose | Status |
|---|--------------|--------|---------|--------|
| B1 | Fine-tuning vs. RAG for training plan generation | OpenRouter models, Anthropic docs, Hugging Face | Can we RAG over a training curriculum corpus, or do we need fine-tuning? | ⬜ TODO |
| B2 | Dog breed image recognition models | Hugging Face model hub, TensorFlow Hub, Stanford Dogs Dataset | Snap a photo → identify breed → personalize plan | ⬜ TODO |
| B3 | Pose estimation for dogs (optional, Phase 2+) | Animal pose estimation papers, BADJA dataset | "Is the dog sitting correctly?" — visual feedback on training exercises | ⬜ TODO |
| B4 | Spanish TTS quality benchmark | ElevenLabs voices, Google Cloud TTS, Azure Neural TTS | Which engine produces the most natural Mexican/neutral Spanish? | ⬜ TODO |
| B5 | App store optimization (ASO) for Spanish keywords | Sensor Tower, App Annie, manual keyword research | "adiestramiento de perros", "entrenar perro", "como educar a mi perro" | ⬜ TODO |
| B6 | React Native vs. Flutter vs. Expo decision | Performance benchmarks, Spanish i18n support, video playback | Framework selection for mobile app | ⬜ TODO |
| B7 | Offline-first architecture patterns | WatermelonDB, Realm, SQLite + sync | Training lessons must work without internet (parks, yards) | ⬜ TODO |

### Phase C: Content Pipeline Research

| # | Research Task | Source | Purpose | Status |
|---|--------------|--------|---------|--------|
| C1 | Video lesson format benchmarks | Analyze top 20 Dogo/Puppr lessons, YouTube training videos | What length, format, pacing works? What do users skip? | ⬜ TODO |
| C2 | AI video generation quality for dog content | fal.ai Kling, Higgsfield, HeyGen with dog training prompts | Can we generate realistic dog training demo videos, or do we need real footage? | ⬜ TODO |
| C3 | Voiceover persona testing | ElevenLabs voice library — test 10 Spanish voices | Find the "trainer voice" — warm, authoritative, encouraging, native Mexican Spanish | ⬜ TODO |
| C4 | Caption/subtitle workflow for bilingual | fal.ai Whisper, manual review workflow | Accuracy of auto-captions in code-switched Spanish/English | ⬜ TODO |

---

## 4. Training Data Strategy

### The Core Question: Fine-Tune a New Model, or RAG Over Existing Knowledge?

**Option A: RAG (Retrieval-Augmented Generation) — RECOMMENDED for MVP**

Why: We don't need to train a model from scratch. Dog training is a well-documented discipline. What we need is:
1. A high-quality **knowledge base** of certified training content (in Spanish)
2. A good **retrieval system** that finds the right lesson for the user's situation
3. A **generation layer** (Claude/GPT via OpenRouter) that personalizes the response

**Data sources for the RAG corpus:**
- AKC training guides (public domain summaries)
- CPDT-KA study materials (purchase or license)
- Karen Pryor "Don't Shoot the Dog" (core clicker training methodology)
- Ian Dunbar "Before & After Getting Your Puppy" (free PDF on his site)
- AVSAB position statements (free, authoritative)
- Spanish translations: hire a certified bilingual dog trainer to review/translate core curriculum (~$2-5K)
- FCI breed standards (free, already in Spanish)
- User-generated Q&A (from beta users — builds over time)

**Option B: Fine-Tune (Phase 2+, only if needed)**

If RAG quality isn't good enough for nuanced behavior modification advice:
- Fine-tune on certified trainer Q&A pairs
- Dataset: 5,000-10,000 (question, answer) pairs from real trainer consultations
- Source: partner with 2-3 certified trainers, pay them to answer 500 questions each ($2-5K total)
- Model: Llama 3 or Mistral fine-tune via Together.ai or Replicate (~$50-200 per training run)

**Our advantage: Printing Press can wrap any ML API.**
- Need a dog breed classifier? Print a CLI for the Hugging Face Inference API
- Need to serve the RAG system? Print a CLI for the Supabase pgvector endpoint
- Need voiceovers? Already have ElevenLabs MCP connected

---

## 5. Architecture & Tech Stack

```
┌─────────────────────────────────────────────────┐
│                   BUEN CHICO                     │
├─────────────────────────────────────────────────┤
│                                                  │
│  ┌──────────┐   ┌──────────┐   ┌──────────┐    │
│  │  Mobile   │   │   Web    │   │  Admin   │    │
│  │  App      │   │  (PWA)   │   │ Dashboard│    │
│  │ (Expo/RN) │   │ (Next.js)│   │ (Next.js)│    │
│  └────┬─────┘   └────┬─────┘   └────┬─────┘    │
│       │              │              │            │
│       └──────────────┼──────────────┘            │
│                      │                           │
│              ┌───────▼────────┐                  │
│              │   Supabase     │                  │
│              │  (PostgreSQL)  │                  │
│              │  + Auth        │                  │
│              │  + pgvector    │                  │
│              │  + Storage     │                  │
│              │  + Edge Fns    │                  │
│              └───────┬────────┘                  │
│                      │                           │
│       ┌──────────────┼──────────────┐            │
│       │              │              │            │
│  ┌────▼─────┐  ┌─────▼────┐  ┌─────▼────┐      │
│  │ AI Engine│  │ Content  │  │ Analytics│      │
│  │(OpenRouter│  │ Pipeline │  │  (GA4 +  │      │
│  │ Claude)  │  │(fal+11L) │  │ PostHog) │      │
│  └──────────┘  └──────────┘  └──────────┘      │
│                                                  │
│  ┌──────────────────────────────────────────┐   │
│  │         Cloudflare (CDN + R2 + Workers)   │   │
│  └──────────────────────────────────────────┘   │
└─────────────────────────────────────────────────┘
```

### Stack Decisions

| Layer | Choice | Why |
|-------|--------|-----|
| **Mobile** | Expo (React Native) | JS ecosystem matches our team skills, Expo manages builds, OTA updates, good i18n |
| **Web** | Next.js on Vercel | Already have Vercel MCP, deploy in minutes |
| **Database** | Supabase PostgreSQL | Already connected, RLS for multi-tenant, pgvector for RAG |
| **Auth** | Supabase Auth | Email + Google + Apple sign-in, free tier handles MVP |
| **AI/LLM** | OpenRouter → Claude 3.5 Sonnet | Multi-model routing, Spanish-capable, cost-effective |
| **RAG** | Supabase pgvector | Embed training corpus → semantic search → feed to LLM |
| **Video CDN** | Cloudflare R2 | $0.015/GB storage, free egress, already connected |
| **Video Gen** | fal.ai (Kling 2.1) | Proven in our content-loop pipeline |
| **Voiceover** | ElevenLabs | 30+ languages, native Spanish voices, already MCP connected |
| **Captions** | fal.ai Whisper | Multilingual transcription, proven in our pipeline |
| **Analytics** | PostHog (self-hosted on Supabase) or GA4 | Event tracking, funnels, A/B testing |
| **Push Notifs** | Expo Push + OneSignal | Free tier handles 10K+ users |
| **Payments** | RevenueCat (wraps App Store + Play Store) | Subscription management, handles both stores |

---

## 6. Tools We Already Have

### Printing Press CLIs (Ready to Deploy)

| CLI | Dog App Use |
|-----|-------------|
| `facebook-graph-pp-cli` | Marketing: post training tips, drive app installs |
| `google-business-profile-pp-cli` | If we open a training business alongside the app |
| `google-search-console-pp-cli` | SEO for companion blog/website |

### Printing Press CLIs (Need to Print)

| CLI | Dog App Use | Priority |
|-----|-------------|----------|
| `supabase-pp-cli` | Type-safe DB queries for user data, lessons, progress | 🔴 HIGH |
| `fal-ai-pp-cli` | Video generation, image gen, Whisper captions | 🔴 HIGH |
| `elevenlabs-pp-cli` | Spanish voiceover generation | 🔴 HIGH |
| `openrouter-pp-cli` | LLM calls for training plan generation | 🔴 HIGH |
| `stripe-pp-cli` | Payment processing (web version) | 🟡 MEDIUM |
| `google-analytics-pp-cli` | User behavior tracking | 🟡 MEDIUM |

### Skills Vault (Directly Reusable)

| Skill | Reuse For |
|-------|-----------|
| `content-loop.md` | Adapt 7-stage video pipeline for training lesson generation |
| `cinematic-blog-and-15s-ad.md` | Marketing video creation for app promo |
| `blog-publish-github-auth.md` | Publish companion training blog |
| `seo-ranking-optimizer.md` | ASO (App Store Optimization) keyword tracking |
| `magic-ad-music-library.md` | Background music for training videos |

### MCP Connections (Already Live)

| MCP | Use |
|-----|-----|
| **Supabase** | Database, auth, storage, vector search |
| **Vercel** | Web deployment, edge functions |
| **Cloudflare** | CDN, R2 video storage, Workers |
| **ElevenLabs** (via Zapier) | Voiceover generation |
| **HeyGen** (via Zapier) | Avatar video for instructor character |
| **Higgsfield** | AI video generation, character training |
| **fal.ai** | Image gen, video gen, Whisper STT |
| **Apollo** | B2B lead gen for trainer partnerships |
| **GitHub** | Code hosting, CI/CD |
| **Google Calendar** | Scheduling (trainer consultations, beta testing) |
| **Gmail** | User communication, beta feedback |

---

## 7. What We Need to Build / Buy

### Must Build

| Item | Effort | Notes |
|------|--------|-------|
| **Supabase schema** (users, dogs, lessons, progress, embeddings) | 2-3 days | Schema design + migrations + RLS policies |
| **RAG pipeline** (embed training corpus → pgvector → retrieval) | 3-5 days | Chunk corpus, generate embeddings, build retrieval function |
| **AI training plan generator** (Edge Function) | 3-5 days | Takes user profile + dog profile → generates personalized curriculum |
| **Expo mobile app** (core screens) | 2-3 weeks | Onboarding, lesson player, progress tracker, AI chat, settings |
| **Content pipeline adaptation** (content-loop → training videos) | 1 week | Modify existing pipeline for dog training demo format |
| **Admin dashboard** (Next.js) | 1 week | Manage lessons, view user analytics, moderate content |

### Must Buy / License

| Item | Cost | Notes |
|------|------|-------|
| **Certified trainer review** (Spanish curriculum) | $2,000-5,000 | Pay a bilingual CPDT-KA to review/translate 100 core lessons |
| **Real dog training footage** (if AI video isn't good enough) | $500-2,000 | Stock footage or hire a trainer for 1 shoot day |
| **Apple Developer Account** | $99/year | Required for App Store |
| **Google Play Developer Account** | $25 one-time | Required for Play Store |
| **RevenueCat** | Free to $0.01/transaction | Subscription management |
| **Expo EAS** | Free tier → $99/mo at scale | Build service for iOS/Android |
| **Domain** | ~$12/year | buenchico.app or buenchico.com |

### Must Research Before Deciding

| Decision | Options | Depends On |
|----------|---------|------------|
| AI video vs. real footage | fal.ai Kling vs. stock footage vs. custom shoot | C2 research (does AI look good enough for dog demos?) |
| Fine-tune vs. RAG | RAG first, fine-tune if quality insufficient | B1 research + beta feedback |
| Dog breed recognition | Hugging Face model vs. user self-reports breed | B2 research (is the model accurate enough to be useful?) |

---

## 8. MVP Scope

### MVP = "Week 1 Dog Owner Experience"

**User story:** Maria downloads Buen Chico. She has a 4-month-old Labrador named Luna who jumps on guests and won't sit. Maria speaks Spanish at home, English at work.

**MVP screens:**
1. **Onboarding** (3 screens) — language preference, dog profile (name, breed, age, weight), top 3 behavior problems
2. **Home / Mi Plan** — personalized 7-day training plan with daily lessons
3. **Lesson Player** — 30-60s video with bilingual voiceover + captions, step-by-step text instructions below
4. **Progress Tracker** — check off completed lessons, upload photo/video of dog performing trick
5. **AI Trainer Chat** — "Mi perro no deja de ladrar cuando llegan visitas" → personalized advice
6. **Settings** — language toggle (ES/EN), notification preferences, subscription management

**MVP lessons (20 core lessons, bilingual):**
1. Nombre (Name recognition)
2. Sentado (Sit)
3. Echado (Down)
4. Quieto (Stay)
5. Ven aquí (Come/Recall)
6. Caminar con correa (Loose leash walking)
7. Deja (Leave it)
8. Suelta (Drop it)
9. Espera (Wait)
10. Toque (Touch/Target)
11. No saltar (No jumping)
12. Entrenamiento de jaula (Crate training)
13. Ir al baño (House training)
14. Socialización (Socialization basics)
15. Mordida suave (Bite inhibition)
16. Quieto en la puerta (Door manners)
17. Saludo educado (Polite greetings)
18. Solo en casa (Alone training / separation)
19. Paseo tranquilo (Calm walks)
20. Juegos de enriquecimiento (Enrichment games)

**NOT in MVP:** breed recognition camera, pose estimation, live trainer video calls, marketplace, social features, advanced behavior modification (aggression, severe anxiety).

---

## 9. Phase Plan

### Phase 0: Research & Planning (NOW — 1-2 weeks)
- [ ] Complete research tasks A1-A6, B1-B7, C1-C4
- [ ] Finalize curriculum outline (20 MVP lessons in Spanish)
- [ ] Identify and contract bilingual certified trainer for content review
- [ ] Domain registration + brand identity (logo, colors, voice)
- [ ] Supabase project creation + schema design
- [ ] Print 4 high-priority CLIs (supabase, fal-ai, elevenlabs, openrouter)

### Phase 1: Core Infrastructure (Week 3-4)
- [ ] Supabase schema deployed (users, dogs, lessons, progress, embeddings)
- [ ] RAG pipeline operational (training corpus embedded, retrieval tested)
- [ ] AI training plan generator (Edge Function) returning personalized plans
- [ ] Expo project scaffolded with i18n (Spanish-first)
- [ ] Auth flow working (Supabase Auth → Expo)

### Phase 2: Content Pipeline (Week 5-6)
- [ ] Adapt content-loop.md for training video generation
- [ ] Generate 20 MVP lesson videos (fal.ai Kling or real footage — based on C2 research)
- [ ] ElevenLabs voiceover for all 20 lessons (Spanish + English tracks)
- [ ] Whisper captions generated and reviewed
- [ ] Videos uploaded to Cloudflare R2

### Phase 3: Mobile App (Week 7-9)
- [ ] Onboarding flow (dog profile creation)
- [ ] Home screen with personalized plan
- [ ] Lesson player (video + text + progress checkbox)
- [ ] AI trainer chat (OpenRouter → Claude, grounded in RAG corpus)
- [ ] Progress tracker with photo upload
- [ ] Settings + language toggle
- [ ] Push notifications (daily lesson reminders)

### Phase 4: Polish & Beta (Week 10-11)
- [ ] RevenueCat subscription integration
- [ ] App Store / Play Store submission prep
- [ ] Beta test with 20-50 Spanish-speaking dog owners
- [ ] Certified trainer reviews all AI-generated advice
- [ ] Performance optimization (offline lesson caching)
- [ ] Analytics instrumentation (PostHog or GA4)

### Phase 5: Launch (Week 12)
- [ ] App Store + Play Store submission
- [ ] Landing page on Vercel (buenchico.app)
- [ ] Launch marketing: Facebook ads targeting Hispanic dog owners in RGV + TX
- [ ] Content marketing: TikTok/Reels training tip clips (repurpose lesson videos)
- [ ] ASO optimization for Spanish keywords

### Phase 6: Growth (Post-Launch)
- [ ] Fine-tune model if RAG quality needs improvement
- [ ] Breed recognition camera feature
- [ ] Advanced behavior modification modules
- [ ] Trainer marketplace (certified trainers offer paid consultations)
- [ ] Social features (share progress, community challenges)
- [ ] Expand to Portuguese (Brazil = 215M people, same gap)

---

## 10. Cost Estimates

### MVP Development (12 weeks)

| Item | Cost |
|------|------|
| Certified trainer (curriculum review) | $3,000 |
| Apple Developer Account | $99 |
| Google Play Developer Account | $25 |
| Domain (buenchico.app) | $15 |
| Supabase (Pro plan) | $25/mo × 3 = $75 |
| Vercel (Pro plan) | $20/mo × 3 = $60 |
| fal.ai (video generation for 20 lessons) | ~$50 |
| ElevenLabs (voiceover for 40 tracks) | ~$22/mo × 3 = $66 |
| OpenRouter (LLM for RAG during dev) | ~$20 |
| Cloudflare R2 (video storage) | ~$5 |
| Expo EAS (free tier during dev) | $0 |
| **Total MVP cost** | **~$3,415** |

### Monthly Operating (Post-Launch, 1K users)

| Item | Cost |
|------|------|
| Supabase Pro | $25 |
| Vercel Pro | $20 |
| OpenRouter (AI chat, ~5K queries/mo) | ~$50 |
| ElevenLabs (new content generation) | $22 |
| Cloudflare R2 + bandwidth | ~$10 |
| RevenueCat | $0 (free under $2.5K MRR) |
| **Total monthly** | **~$127/mo** |

### Revenue Model

| Plan | Price | Target |
|------|-------|--------|
| Free | $0 | 5 lessons, limited AI chat (3/day) |
| Premium | $4.99/mo | All lessons, unlimited AI chat, progress tracking |
| Annual | $29.99/yr | Same as Premium, 50% discount |
| Family | $7.99/mo | Up to 5 dog profiles |

**Break-even:** ~26 Premium subscribers ($127 / $4.99)
**Target Month 6:** 500 paid subscribers = $2,495 MRR

---

## 11. Open Questions

1. **Brand name confirmed?** "Buen Chico" (Good Boy) — catchy, bilingual-friendly, .app available?
2. **Trainer partnership:** Do we hire one certified trainer on retainer, or contract multiple for content diversity?
3. **AI video quality:** Can fal.ai Kling generate realistic enough dog training demos, or do we need real footage? (Research task C2 will answer this)
4. **Monetization timing:** Launch free-only for 30 days to build reviews, then gate premium? Or paywall from day 1?
5. **Target market geography:** Start with RGV/Texas (local marketing advantage) or go national from day 1?
6. **Portuguese expansion timeline:** If Spanish MVP works, Brazil is the obvious next market — when do we plan for it?
7. **Regulatory:** Any app store policies about animal training advice we need to be aware of? Disclaimer language?

---

## Appendix: Printing Press Print Queue

These CLIs need to be generated using our Printing Press pipeline before development begins:

```bash
# Priority 1 — Core app backend
printing-press print supabase-management-api
printing-press print fal-ai
printing-press print elevenlabs
printing-press print openrouter

# Priority 2 — Growth tools
printing-press print revenucat
printing-press print onesignal
printing-press print posthog

# Priority 3 — Marketing
printing-press print sensor-tower    # ASO tracking
printing-press print app-store-connect  # iOS analytics
printing-press print google-play-console  # Android analytics
```

---

*Last updated: 2026-05-11*
*Repo: https://github.com/mar2181/buen-chico*
