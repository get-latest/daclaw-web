# Managed LLM Tokens for SnappyClaw.ai — Research Summary

> **Goal:** Understand how SnappyClaw.ai can offer managed LLM tokens (bundled AI usage) as part of subscription tiers.
> **Tiers under consideration:** $29/mo (500 messages) → $119/mo (10,000 messages)
> **Last updated:** February 24, 2026

---

## Table of Contents
1. [Bulk/Reseller Pricing by Provider](#1-bulkreseller-pricing-by-provider)
2. [API Aggregators](#2-api-aggregators)
3. [Unit Economics](#3-unit-economics)
4. [Legal & TOS Considerations](#4-legal--tos-considerations)
5. [Recommended Approach](#5-recommended-approach)

---

## 1. Bulk/Reseller Pricing by Provider

None of the major LLM providers have formal "reseller" programs in the traditional SaaS channel sense. Volume discounts exist but require direct negotiation once you hit meaningful spend thresholds. Here's what's available:

### Anthropic (Claude)

**Standard API pricing (per 1M tokens, Feb 2026):**

| Model | Input | Output | Notes |
|-------|-------|--------|-------|
| Claude Haiku 4.5 | $1.00 | $5.00 | Best cost/performance ratio for high volume |
| Claude Haiku 3.5 | $0.80 | $4.00 | Budget option, still solid |
| Claude Sonnet 4.5 | $3.00 | $15.00 | Balanced quality |
| Claude Opus 4.5 | $5.00 | $25.00 | Flagship |
| Claude Opus 4.1 (legacy) | $15.00 | $75.00 | Very expensive |

**Cost-cutting features:**
- **Batch API:** 50% discount (async, results within 24h)
- **Prompt caching:** 90% savings on repeated context (cache read: $0.10/MTok for Haiku 4.5). Great for system prompts and conversation history.

**Bulk pricing:**
- No public reseller program
- Custom pricing negotiated at **~$100K+ annual API spend**
- Contact: sales@anthropic.com — typically responds within a few business days
- Minimum commitment not publicly stated but typically $50K-100K/year to start discussions

### OpenAI (GPT)

**Standard API pricing (per 1M tokens, Feb 2026):**

| Model | Input | Output | Notes |
|-------|-------|--------|-------|
| GPT-5 mini | $0.25 | $2.00 | Best quality-to-cost for personal assistant |
| GPT-4.1 nano | $0.20 | $0.80 | Cheap, good for simple tasks |
| GPT-4.1 | $3.00 | $12.00 | High-quality balanced model |
| GPT-5.2 | $1.75 | $14.00 | Flagship, surprisingly affordable input |
| GPT-5.2 Pro | $21.00 | $168.00 | Ultra-premium, avoid for consumer products |

**Cost-cutting features:**
- **Batch API:** 50% discount (async)
- **Cached inputs:** 90% discount on repeated context

**Bulk pricing:**
- OpenAI Enterprise: Custom pricing for large customers, typically negotiated at $250K+ annual spend
- No formal reseller program — you're building on top as a developer

### Google (Gemini)

**Standard API pricing (per 1M tokens, Feb 2026):**

| Model | Input | Output | Notes |
|-------|-------|--------|-------|
| Gemini 2.5 Flash Lite | $0.10 | $0.40 | Incredible value, still capable |
| Gemini 2.5 Flash | $0.30 | $2.50 | Best quality-to-cost among major models |
| Gemini 2.5 Pro | $1.25–2.50 | $10–15 | Premium (tiered by context size) |

**Route:** Two options:
1. **Google AI Studio API** (direct, simpler, developer-friendly)
2. **Google Vertex AI** (enterprise-grade, GCP-integrated, SLAs)

**Bulk pricing via Vertex AI:**
- Committed use discounts available through Google Cloud contracts
- Vertex AI gives access to enterprise SLAs, IAM controls, regional deployment
- GCP Marketplace deals possible at scale
- Contact Google Cloud sales for custom pricing — Google is aggressively discounting to compete

### Mistral AI

**Standard API pricing (per 1M tokens, Feb 2026):**

| Model | Input | Output | Notes |
|-------|-------|--------|-------|
| Mistral NeMo | $0.15 | $0.15 | Ultra-cheap, open-weight |
| Mistral Small 3.1 | $0.10 | $0.30 | Cheapest capable model |
| Mistral Medium 3 | $0.40 | $2.00 | Excellent value, strong quality |
| Mistral Large | $2.00 | $6.00 | Premium |

**Bulk pricing:**
- Custom enterprise contracts starting at "six figures" — but Mistral is the most negotiation-friendly of all the big players
- Good option for budget-conscious products
- EU-based (GDPR-native), which matters for some markets

### DeepSeek (Bonus Wild Card)

| Model | Input | Output | Notes |
|-------|-------|--------|-------|
| DeepSeek V3 | $0.28 | $0.42 | Jaw-dropping value |
| (cache hit) | $0.028 | — | Nearly free with caching |

DeepSeek is a Chinese AI company — impressive quality-to-cost ratio but comes with data privacy / sovereignty concerns that may be a dealbreaker for US SaaS targeting US customers.

---

## 2. API Aggregators

### OpenRouter

**What it is:** A unified API that routes to 400+ models across all providers. No markup — passes through provider pricing exactly.

**Pros:**
- ✅ Zero markup on inference pricing
- ✅ Single API key for Claude, GPT, Gemini, Mistral, Llama, etc.
- ✅ Easy model switching without code changes
- ✅ BYOK (Bring Your Own Key) — 1M free requests/month using your own provider keys
- ✅ Enterprise plan with SOC2 compliance, SLAs, centralized billing
- ✅ Fallback routing across providers for better uptime
- ✅ Usage analytics built in

**Cons:**
- ❌ Another intermediary in your stack (latency +10-30ms)
- ❌ Credits-based billing (pay in advance)
- ❌ Not as established for enterprise support as AWS/Azure/GCP

**Best for:** Early-stage startups that want to move fast, avoid lock-in, and experiment with multiple models.

**BYOK option:** If you supply your own API keys for each provider, OpenRouter routes for free (up to 1M requests/month). This is an excellent path for early stage.

---

### AWS Bedrock

**What it is:** AWS's managed AI platform — hosts models from Anthropic, Mistral, Meta, Stability AI, Amazon, and others.

**Pricing:** Same per-token rates as going direct to Anthropic/Mistral, with no markup. You pay AWS standard rates.

**Pros:**
- ✅ SOC2, HIPAA, PCI compliance built in
- ✅ Data never leaves your AWS region
- ✅ No separate billing — consolidated AWS invoice
- ✅ AWS Bedrock Guardrails for safety filtering
- ✅ Enterprise SLAs
- ✅ Intelligent prompt routing (auto-route to cheaper models for simpler queries)
- ✅ Volume discounts via AWS committed use contracts
- ✅ If you're already on AWS, this is natural

**Cons:**
- ❌ More complex setup than direct API
- ❌ AWS dependency/complexity
- ❌ Not all models available (no OpenAI GPT on Bedrock — that's Azure-only)
- ❌ Anthropic's own TOS on Bedrock explicitly prohibits reselling without approval (important — see section 4)

**Best for:** Companies scaling past $10K/mo in API costs who need enterprise features.

---

### Google Vertex AI

**What it is:** Google Cloud's enterprise AI platform. Best path for Gemini models at scale.

**Pros:**
- ✅ Native access to all Gemini models with enterprise SLAs
- ✅ Committed use discounts via GCP
- ✅ Regional data residency options
- ✅ Excellent for multimodal (images, audio, video)
- ✅ Google Search grounding available (unique feature)
- ✅ Tight integration with BigQuery, Cloud Storage

**Cons:**
- ❌ GCP-dependent
- ❌ Gemini models only (no Claude or GPT natively)
- ❌ More complex than direct Gemini API

**Best for:** If Gemini Flash is your primary model AND you're serious about scale.

---

### Azure OpenAI Service

**What it is:** Microsoft Azure's hosted version of OpenAI models. The ONLY official way to use GPT models with enterprise SLAs.

**Pros:**
- ✅ Only enterprise-grade route for GPT models
- ✅ SOC2/HIPAA/PCI compliance
- ✅ Data doesn't train OpenAI models
- ✅ Provisioned throughput options (reserved capacity for predictable latency)
- ✅ Azure credits programs for startups

**Cons:**
- ❌ Azure-only (lock-in)
- ❌ GPT models only (no Claude or Gemini)
- ❌ Approval process required (manual review, takes 1-5 days)
- ❌ Pricing parity with OpenAI direct, no discount for small users

**Best for:** Enterprise customers who require Azure compliance, or if you're building on the Microsoft stack.

---

## 3. Unit Economics

### Assumptions

For a personal AI assistant:
- **Average message:** ~800 input tokens (user query + recent conversation context) + ~600 output tokens (assistant response) = ~1,400 tokens per message
- This is a realistic average — short chats are cheaper, long documents more expensive
- With smart prompt caching on system prompts, actual input costs can drop 30-60%

### Cost Per Message by Model

| Model | Cost/msg | Provider |
|-------|----------|----------|
| Mistral Small 3.1 | **$0.00026** | Mistral |
| Gemini 2.5 Flash Lite | **$0.00032** | Google |
| GPT-4.1 nano | **$0.00064** | OpenAI |
| Mistral Medium 3 | **$0.00152** | Mistral |
| Gemini 2.5 Flash | **$0.00174** | Google |
| GPT-5 mini | **$0.00140** | OpenAI |
| Claude Haiku 4.5 | **$0.00380** | Anthropic |
| Claude Sonnet 4.5 | **$0.01140** | Anthropic |
| GPT-4.1 | **$0.00960** | OpenAI |
| Claude Opus 4.5 | **$0.01900** | Anthropic |

### COGS Analysis Per Subscription Tier

#### Tier 1: $29/mo — 500 messages

| Model | COGS | Gross Margin |
|-------|------|-------------|
| Mistral Small 3.1 | $0.13 | **99.6%** 🟢 |
| Gemini 2.5 Flash Lite | $0.16 | **99.4%** 🟢 |
| GPT-5 mini | $0.70 | **97.6%** 🟢 |
| Mistral Medium 3 | $0.76 | **97.4%** 🟢 |
| Gemini 2.5 Flash | $0.87 | **97.0%** 🟢 |
| Claude Haiku 4.5 | $1.90 | **93.4%** 🟢 |
| Claude Sonnet 4.5 | $5.70 | **80.3%** 🟡 |
| GPT-4.1 | $4.80 | **83.4%** 🟡 |

**All models are economically viable at 500 messages/$29.**

#### Tier 2: $59/mo — 2,000 messages

| Model | COGS | Gross Margin |
|-------|------|-------------|
| Mistral Small 3.1 | $0.52 | **99.1%** 🟢 |
| Gemini 2.5 Flash Lite | $0.64 | **98.9%** 🟢 |
| GPT-5 mini | $2.80 | **95.3%** 🟢 |
| Mistral Medium 3 | $3.04 | **94.8%** 🟢 |
| Gemini 2.5 Flash | $3.48 | **94.1%** 🟢 |
| Claude Haiku 4.5 | $7.60 | **87.1%** 🟢 |
| Claude Sonnet 4.5 | $22.80 | **61.4%** 🟡 |
| GPT-4.1 | $19.20 | **67.5%** 🟡 |

#### Tier 3: $119/mo — 10,000 messages

| Model | COGS | Gross Margin |
|-------|------|-------------|
| Mistral Small 3.1 | $2.60 | **97.8%** 🟢 |
| Gemini 2.5 Flash Lite | $3.20 | **97.3%** 🟢 |
| GPT-5 mini | $14.00 | **88.2%** 🟢 |
| Mistral Medium 3 | $15.20 | **87.2%** 🟢 |
| Gemini 2.5 Flash | $17.40 | **85.4%** 🟢 |
| Claude Haiku 4.5 | $38.00 | **68.1%** 🟡 |
| Claude Sonnet 4.5 | $114.00 | **4.2%** 🔴 LOSS RISK |
| GPT-4.1 | $96.00 | **19.3%** 🔴 TERRIBLE |

### Key Takeaways on Unit Economics

1. **Claude Sonnet at 10,000 messages is a business killer** — nearly breakeven before any infra, support, or overhead costs.
2. **The safe sweet spot for quality + economics:** Gemini 2.5 Flash or GPT-5 mini — both deliver genuinely good quality at ~85-95% gross margin even at the highest tier.
3. **If you want to offer Claude branding:** Use Haiku 4.5 (68% margin at 10K msgs is workable), NOT Sonnet.
4. **Smart routing is the secret weapon:** Use a cheap model (Mistral Small, Gemini Flash Lite) for simple/short messages and a better model (Gemini Flash, GPT-5 mini) for longer/complex ones. This can cut COGS by 40-60%.
5. **Prompt caching is a major lever** — if your system prompt is 1000+ tokens (common for personal assistants), caching it reduces effective input costs by 60-90%.

### Quality-to-Cost Ranking for Personal AI Assistant

1. **🏆 Gemini 2.5 Flash** — Best balance. Strong reasoning, great context window, cheap. The right default model for most consumer AI products in 2026.
2. **🥈 GPT-5 mini** — Excellent quality, OpenAI brand recognition, very affordable.
3. **🥉 Mistral Medium 3** — European privacy-friendly option. Surprisingly capable at $0.40/$2.
4. **Claude Haiku 4.5** — Good quality but 2-3x more expensive than the above. Worth it if Anthropic brand matters.
5. **Claude Sonnet** — Only viable at low message counts or very high price points.

---

## 4. Legal & TOS Considerations

### The Core Distinction: "Building on" vs. "Reselling"

This is the most important thing to understand. Every major LLM provider prohibits **reselling the API as-is** but explicitly **permits building products on top**. The line:

| ❌ NOT ALLOWED | ✅ ALLOWED |
|----------------|-----------|
| Selling raw API keys to users | Building a product that uses the API internally |
| Creating a proxy that just relays API calls | Adding your own UI, system prompts, memory, workflows |
| Offering "AI credits" that map 1:1 to tokens | Offering "messages" with value-added features |
| Subscription plans that expose the underlying API | Subscription plans for your product that happen to use AI |

**In plain English:** SnappyClaw can absolutely bundle AI usage into subscriptions. You're building a personal AI assistant product — that's value-add. You're not selling "Claude API credits" or providing raw API access.

### Anthropic's TOS

**Key clause:** *"Customer may not and must not attempt to access the Services to build a competing product or service, including to train competing AI models or resell the Services except as expressly approved by Anthropic."*

**What this means for SnappyClaw:**
- ✅ Building a personal AI assistant product = fine
- ✅ Bundling message quotas into subscription tiers = fine
- ❌ Letting users run arbitrary API calls through your key = problematic
- ❌ Offering "tokens" that users can spend on anything = borderline

**Recent enforcement precedent:** Anthropic blocked "Claude Max" from being used in OpenCode (a coding tool that was essentially proxying Claude's subscription to arbitrary tools). This wasn't about the API — it was about subscription plan abuse. The API TOS is less strict than subscription TOS.

**On AWS Bedrock specifically:** The Bedrock commercial terms explicitly state that reselling requires "express approval" from Anthropic — this means you need Anthropic sign-off if you want to resell via Bedrock. Go through the direct Anthropic API instead, which is broader in its commercial use permissions.

### OpenAI's TOS

**Key restriction:** Similar to Anthropic — prohibits reselling API access itself, but explicitly allows commercial products built on top.

- ✅ Commercial SaaS products using GPT = fully allowed
- ✅ Charging subscribers for an AI product = fully allowed
- ❌ Building a competing AI model using outputs = not allowed
- ❌ Selling direct API access = not allowed

OpenAI has explicit commercial use rights for API subscribers. The outputs belong to you (the developer) and can be sold as part of your product.

### Google Gemini TOS

Google is the most permissive of the major providers for commercial use:
- ✅ Commercial products = explicitly permitted
- ✅ Bundling usage into subscriptions = fine
- Vertex AI commercial terms are even cleaner for enterprise use

### Mistral TOS

- Very permissive — most startup-friendly of the major providers
- EU-based (GDPR-native): data stays in EU by default
- No special restrictions on building commercial products

### Practical Bottom Line for SnappyClaw

**You're safe to proceed.** Building a personal AI assistant subscription product that includes bundled message quotas is:
- Universally permitted by all major LLM providers
- Standard industry practice (Cursor, Perplexity, Character.ai, Jasper, and hundreds more do this)
- Only illegal if you literally resell API keys or create a pure proxy

**One caveat:** Keep your users from using SnappyClaw to make raw API calls or as a general-purpose "talk to Claude" proxy. Add product value — memory, personas, integrations, workflows. This keeps you firmly in the "value-add product" camp vs. the "reseller" camp, both legally and competitively.

---

## 5. Recommended Approach

### Phase 1: Early Stage (0-1,000 users, ~$5K/mo revenue)

**Infrastructure:** Use **OpenRouter** with BYOK  
**Primary model:** **Gemini 2.5 Flash** (via Google AI Studio key)  
**Backup model:** **GPT-5 mini** (via OpenAI key)

**Why:**
- Zero setup complexity — one API with two provider keys
- No commitments, no contracts
- 1M free BYOK requests/month saves money
- Gemini 2.5 Flash gives 85%+ gross margin even at the highest tier
- OpenRouter handles failover automatically

**Pricing strategy:**
- $29/mo — 500 messages — ~$0.87 COGS — **97% margin**
- $79/mo — 3,000 messages — ~$5.22 COGS — **93.4% margin**
- $119/mo — 10,000 messages — ~$17.40 COGS — **85.4% margin**

**Important:** These are pure inference costs. Add 20-30% overhead for infrastructure, support, and R&D. Even so, you're at 55-65% gross margin minimum — healthy for a SaaS.

---

### Phase 2: Growth Stage (1,000-10,000 users, $50K-500K ARR)

**Infrastructure:** Move to **direct API access** with multi-model routing  
**Architecture:** 
- Gemini 2.5 Flash Lite for short/simple messages (<500 tokens input)
- Gemini 2.5 Flash for medium complexity
- Claude Haiku 4.5 or GPT-5 mini for complex/long responses
- Add prompt caching for system prompts (critical — saves 40-60% on repeated context)

**Bulk pricing discussions:**
- At $10K+/mo spend with Anthropic → reach out for volume pricing discussion
- Apply for Google Cloud startup credits (often $100K-200K available)
- Apply for OpenAI startup program

**Introduce usage limits** on lower tiers (soft limits via throttling, not hard cutoffs — better UX) and overage charges on power users.

---

### Phase 3: Scale ($500K+ ARR)

**Infrastructure:** Move to **AWS Bedrock** (for Anthropic/Mistral) + **Google Vertex AI** (for Gemini)

**Why:**
- Enterprise SLAs for B2B customers
- SOC2/HIPAA compliance
- Committed use contracts = volume discounts
- Consolidated billing with existing AWS/GCP spend

**Model strategy at scale:**
- Negotiate custom pricing with Anthropic (target $100K+ annual minimum)
- Committed use contracts on Vertex AI
- Fine-tune a Mistral Medium model on your data for even cheaper inference
- Consider hosting open-source models (Llama 3.3, Mistral) on GPU infrastructure for your cheapest tier

---

### Model Selection Decision Tree

```
User sends message
↓
Is it a simple/short message (<400 tokens)?
  YES → Gemini 2.5 Flash Lite ($0.00032/msg) ← default for most messages
  NO ↓
Is it a complex task (coding, analysis, long document)?
  YES → GPT-5 mini or Gemini 2.5 Flash ($0.0014-0.0017/msg)
  NO → Gemini 2.5 Flash ($0.00174/msg) ← safe default
  
Is the user on a "Premium" tier that advertises Claude?
  YES → Claude Haiku 4.5 ($0.0038/msg) ← only if you've priced for it
```

---

### Quick Reference: What to Do Tomorrow

1. **Set up OpenRouter account** → Add Gemini Flash (Google AI Studio key) + GPT-5 mini (OpenAI key) as BYOK providers
2. **Model:** Default to `google/gemini-2.5-flash` for all messages
3. **Implement prompt caching** on your system prompt immediately (50-90% cost savings)
4. **Cap context window** — truncate conversation history to last 5-10 turns for most users (prevents runaway costs on chatty users)
5. **Add usage tracking** per-user, per-month from day one (you'll need this data for pricing optimization)
6. **Build in model switching** at the architecture level — abstract the model behind a service layer so you can swap without code changes

---

### Risk Flags to Monitor

- **Power users:** 5% of users often consume 50% of compute. Implement soft rate limits.
- **Long conversations:** Each turn adds context, increasing input tokens exponentially. Truncate history.
- **Model pricing drops:** AI compute costs have dropped ~70% per year. This helps you — but build flexibility to switch models as costs evolve.
- **Provider outages:** Always have a fallback model (this is where OpenRouter shines early on).
- **Anthropic TOS changes:** They're tightening enforcement. Ensure your product has genuine value-add beyond raw API relay.

---

*Research compiled by Joi for SnappyClaw.ai, February 2026. Pricing current as of research date — verify directly with providers before making financial commitments.*
