# PrepLens AI — Cost, Profitability, and Customer Value Case

## 1. Purpose

This note summarizes the current PrepLens business case for an India-first launch and an illustrative global scale-up. It also explains why a candidate may choose PrepLens over higher-priced interview-preparation alternatives.

> **Important:** The financial figures below are planning assumptions for the AI Product Management capstone. They are not realized financial results. Conversion, token usage, acquisition cost, support cost, and product mix must be validated through a live pilot.

---

## 2. Current PrepLens Pricing

| Plan | Current Price | Primary Value |
|---|---:|---|
| Basic Prep | ₹30 | Low-cost role-specific preparation |
| Pro Prep | ₹60 | Deeper structured preparation and model answers |
| AI Mock Interview | ₹100 | Practice-oriented AI interview simulation and feedback |

For financial modelling, the assumed paid-user mix is:

- 40% Basic
- 40% Pro
- 20% AI Mock Interview

This produces an illustrative blended revenue per paid user of:

`(40% × ₹30) + (40% × ₹60) + (20% × ₹100) = ₹56`

---

# 3. India-First Scale Scenario

## Assumptions

- Monthly users: **50,000**
- Free-to-paid conversion: **10%**
- Paid users: **5,000**
- Non-paying users receiving the free preview: **45,000**
- Blended paid revenue per user: **₹56**
- Monthly marketing / acquisition budget: **₹1,00,000**
- Payment gateway fee: **1% of paid revenue**
- Domain + hosting: **₹2,000 per month**
- Free-preview LLM cost assumption: **~₹0.67 per free user**
- Additional paid-user AI / speech cost: **~₹3.28 per paid user**, blended across product types

The LLM figures are operating assumptions based on a token-priced commercial foundation-model benchmark. The production architecture should remain model-flexible so lower-cost models can be substituted where quality remains acceptable.

## Revenue

5,000 paid users × ₹56 blended revenue:

**Monthly revenue = ₹2,80,000**

## Monthly Cost Model

| Cost Item | Calculation | Estimated Monthly Cost |
|---|---|---:|
| Marketing / customer acquisition | Planned monthly budget | **₹1,00,000** |
| Free-user LLM cost | 45,000 × ~₹0.67 | **₹30,150** |
| Paid-user AI + STT | 5,000 × ~₹3.28 | **₹16,400** |
| Payment gateway | 1% × ₹2,80,000 | **₹2,800** |
| Domain + hosting | Fixed monthly assumption | **₹2,000** |
| **Total monthly operating cost** | | **₹1,51,350** |

## Estimated Operating Surplus

**₹2,80,000 revenue − ₹1,51,350 operating cost = ₹1,28,650 per month**

Rounded presentation number:

### **≈ ₹1.29 lakh monthly operating surplus**

Approximate operating contribution:

**₹1,28,650 ÷ ₹2,80,000 ≈ 46%**

This is before salaries, taxes, refunds, accounting/legal costs, and additional future support overhead.

---

# 4. Illustrative Global Scale Scenario

## Assumptions

- Monthly users: **200,000**
- Conversion rate: **10%**
- Paid users: **20,000**
- Non-paying users: **180,000**
- Illustrative localized pricing:
  - Basic: **$2.99**
  - Pro: **$5.99**
  - AI Mock Interview: **$9.99**
- Assumed product mix: 40% / 40% / 20%
- Blended revenue per paid user: **~$5.59**
- Marketing / acquisition: **$30,000 per month**
- Payment gateway: **1% of paid revenue**
- Cloud + localization + support: **$3,000 per month**
- Free-user LLM cost: **~$0.008 per preview**
- Additional paid AI + STT cost: **~$0.039 per paid user**

## Revenue

20,000 paid users × ~$5.59:

### **~$111,800 monthly revenue**

## Monthly Cost Model

| Cost Item | Calculation | Estimated Monthly Cost |
|---|---|---:|
| Marketing / acquisition | Planned monthly budget | **$30,000** |
| Free-user LLM | 180,000 previews | **~$1,440** |
| Paid-user AI + STT | 20,000 paid users | **~$780** |
| Payment gateway | 1% × $111,800 | **~$1,118** |
| Cloud + localization + support | Fixed planning assumption | **$3,000** |
| **Total monthly operating cost** | | **~$36,338** |

## Estimated Operating Surplus

**$111,800 − $36,338 = ~$75,462 per month**

Rounded presentation number:

### **≈ $75.5K monthly operating surplus**

Approximate operating contribution:

**~67.5%**, before salaries, taxes, refunds, compliance, and other corporate overhead.

---

# 5. Why a Candidate Would Pay for PrepLens

PrepLens is positioned as a **low-friction, self-serve preparation product**, not as a replacement for every form of human coaching.

Its customer-value proposition is based on four factors:

1. **Very low entry price** — ₹30–₹100 rather than committing to a large package.
2. **Immediate availability** — no need to schedule a mentor or interviewer.
3. **Candidate-specific context** — preparation can use the candidate's resume, target role, and job description rather than only generic questions.
4. **Event-driven purchase** — a candidate can pay only when an interview is approaching rather than subscribing for months.

## Current Market Benchmarks

Publicly available India pricing shows that interview-preparation alternatives span a wide range:

| Market Alternative | Observed Pricing | What the User Typically Buys |
|---|---:|---|
| Topmate interview-prep packages | Median around **₹1,270**, with many sessions/packages between roughly ₹487 and ₹2,692 | Human-led interview preparation packages / sessions |
| Topmate mock interview package format | Median around **₹2,500** | Multiple human-led sessions |
| Skilleaze 1:1 interview preparation | **₹2,000** promotional price; listed normal price ₹4,000 | One-hour 1:1 coaching |
| MockNinja | **₹3,500** solo session; **₹5,500** dual-panel session | 45-minute human-led mock interview |
| Mark Consultant | **₹4,999** | Two 45-minute mocks + feedback and coaching |
| Interview EDGE | **₹999–₹2,999** | Questions, answers, or multiple questionnaires |
| Interview Prep Guru | **₹599/month Plus; ₹999/month Pro** | Recurring AI interview-practice subscription |
| Resumo | **₹999 for 10 mocks; ₹1,999 for 30 mocks** | AI voice mock packages |

These benchmarks show that the market contains both lower-cost AI tools and much higher-cost human coaching. Therefore, PrepLens should **not** claim that every competitor costs ₹3,000–₹5,000. A more defensible positioning is:

> **Human-led mock interviews and coaching can cost from hundreds of rupees to several thousand rupees per session or package, while AI interview-prep subscriptions/packages commonly cost hundreds to low-thousands of rupees. PrepLens aims to reduce the entry barrier further with ₹30–₹100 event-based purchases.**

---

# 6. Customer Value Comparison

| Customer Question | Typical Higher-Cost Alternative | PrepLens Positioning |
|---|---|---|
| "I only have an interview tomorrow. Do I need a full course?" | Package or subscription may cost ₹599–₹2,999+ | **₹30–₹100 one-time purchase** |
| "Can I prepare for this exact JD?" | Generic question banks may not use candidate context | **Resume + JD-based contextual preparation** |
| "Can I practice without booking a mentor?" | Human mock requires scheduling | **On-demand AI practice** |
| "I want repeated practice but cannot spend ₹2K–₹5K." | Human coaching can become expensive when repeated | **Low-cost digital repetition** |
| "Do I still need a human coach?" | Human coaching provides richer expert judgement | PrepLens can be used first; human coaching remains valuable for high-stakes cases |

---

# 7. Pricing Logic

PrepLens does not need to prove that it is always better than a human coach. The stronger product argument is:

### **It serves a different price-access point.**

A candidate who is willing to spend ₹2,000–₹5,000 for personalized human coaching may still prefer a human expert for a high-stakes interview.

But a much larger group may want:

- quick preparation,
- role-specific questions,
- structured answer guidance,
- practice immediately before an interview,
- and a price low enough to purchase repeatedly for multiple job applications.

This creates room for PrepLens as the **self-serve layer between free generic content and premium human coaching**.

---

# 8. Investor Interpretation

The investment thesis is not simply that LLM tokens are cheap.

The thesis is that PrepLens combines:

- a recurring pool of job seekers,
- very low customer entry pricing,
- a freemium acquisition funnel,
- API-based variable infrastructure,
- low marginal delivery cost,
- and the ability to localize pricing when entering new markets.

### India illustration

**50K monthly users → 10% conversion → 5K paid → ₹2.80L revenue → ~₹1.29L operating surplus**

### Global illustration

**200K monthly users → 10% conversion → 20K paid → ~$111.8K revenue → ~$75.5K operating surplus**

Both scenarios are hypotheses to validate through pilot data, especially:

- free-to-paid conversion,
- actual token consumption,
- product mix,
- mock-interview duration,
- CAC / marketing efficiency,
- repeat purchases,
- refund rate,
- and customer willingness to pay.

---

# 9. Recommended Presentation Statement

> **PrepLens is not trying to replace ₹3,000–₹5,000 human coaching with a ₹100 AI clone. It targets the much larger self-serve preparation layer. Candidates can start at ₹30, prepare against their actual role and resume, and use an AI mock interview for ₹100 when they need practice. Human-led alternatives can cost hundreds to several thousand rupees per session or package, while subscription tools commonly cost hundreds to low-thousands. This creates a clear affordability and convenience wedge for PrepLens.**

