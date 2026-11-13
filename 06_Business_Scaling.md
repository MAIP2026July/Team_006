# Deliverable 06: Business, Economics, and Scaling View

## 1. Monetization Strategy & Pricing Architecture

PrepLens uses an **event-driven, one-time purchase model** rather than a mandatory subscription. The current live-product pricing is:

| Product | Current Price | Primary Value |
| :--- | ---: | :--- |
| **Free Preview** | **₹0** | Sample contextual questions that demonstrate personalization before purchase. |
| **Basic Prep Guide** | **₹30** | Entry-level paid preparation for candidates who want more depth than the free preview. |
| **Pro Prep Guide** | **₹60** | Deeper preparation content, examples, and structured guidance. |
| **AI Mock Interview** | **₹100** | Practice under interview-like pressure with AI-led questioning and feedback. |

The commercial hypothesis is that interview preparation is **episodic**: users are more likely to pay around an upcoming interview than commit to a recurring monthly subscription. This is a **product hypothesis to validate in the pilot**, not yet a proven behavioral fact.

### Product Funnel

```text
Free contextual preview
        ↓
Candidate sees evidence of personalization
        ↓
Chooses required depth
   ├── Basic ₹30
   ├── Pro ₹60
   └── AI Mock Interview ₹100
        ↓
Measure value, completion, repeat purchase and cost-to-serve by path
```

---

## 2. Unit Economics: Pilot Measurement Model

The earlier design used a single ₹40 bundle. The product has since evolved to three separate paid paths, so the correct PM approach is to measure **contribution margin by workflow**, not assume one margin for every user.

### Contribution Formula

> **Contribution per completed journey = Price − payment fees − model/API cost − speech cost − hosting/egress − support/refund allocation**

| Workflow | Revenue | Main Variable Cost Drivers | Pilot Question |
| :--- | ---: | :--- | :--- |
| **Basic Guide** | ₹30 | Payment fee, text-generation tokens, hosting | Can this remain attractive after transaction and inference costs? |
| **Pro Guide** | ₹60 | Higher text-generation volume, payment fee, hosting | Does the additional depth improve conversion/value enough to justify higher generation cost? |
| **AI Mock Interview** | ₹100 | Speech-to-text minutes, LLM follow-up turns, hosting, payment fee | Can the most compute-intensive workflow maintain a healthy contribution after audio and follow-up usage? |

### Important Evidence Boundary

The cost figures in this capstone are **planning assumptions / target-cost models unless supported by production logs**. They should not be presented as observed gross margin. During pilot, PrepLens should capture actual API usage, audio minutes, payment fees, refunds, and hosting consumption for each completed journey.

---

## 3. Scaling Guardrails

### Primary Cost Risk: Unbounded AI Mock Usage

Voice interactions are expected to be the most variable cost component because cost grows with **audio duration and follow-up turns**. The product therefore uses quota controls such as:

* Maximum questions per mock session.
* Maximum recording duration per answer/session.
* Token/output caps for AI feedback.
* Clear handling of retries so failed requests do not silently multiply cost.
* Separate telemetry for Basic, Pro, and Mock Interview cost-to-serve.

These guardrails protect the business while also making the user experience predictable.

---

## 4. Acquisition, Retention, and LTV/CAC — Hypotheses to Validate

Potential low-cost acquisition channels include campus-placement communities, student ambassadors, organic role-specific content, referrals, and targeted social distribution.

The earlier artifact used specific CAC, repeat-purchase and LTV:CAC figures as if they were established. In the revised plan these are treated correctly as **pilot hypotheses**.

| Metric | Pilot Measurement |
| :--- | :--- |
| **CAC** | Actual acquisition spend ÷ newly acquired paying customers, by channel. |
| **Repeat Purchase Rate** | % of paid users purchasing again for another role/interview. |
| **Revenue per Paying User** | Total paid revenue ÷ unique paying users. |
| **Contribution per Paying User** | Revenue minus directly attributable variable costs. |
| **LTV:CAC** | Calculated only after enough repeat-purchase and retention evidence exists. |

**Decision rule:** do not claim a healthy LTV:CAC ratio until repeat purchase and acquisition economics are observed in a sufficiently sized pilot cohort.

---

## 5. Scaling Constraints & Operational Bottlenecks

1. **Model/API Rate Limits and Latency:** Peak placement periods may create bursts of generation demand. Queueing, graceful progress states, retries, and provider fallbacks should be tested before scaling.
2. **Payment Failures / Drop-Off:** Track checkout initiation, completion, failure and retry by package rather than assuming all free users convert similarly.
3. **Voice Cost Variability:** Mock-interview margins can deteriorate if duration and follow-up turns are unconstrained; session quotas and cost alerts are therefore launch controls.
4. **Domain Coverage Drift:** Expansion beyond IT/software requires domain-specific evaluation sets and prompt/rubric tuning before claiming equivalent quality.
5. **Support and Refund Load:** Growth can improve revenue while still weakening economics if failed generations, payment disputes or support demand grow faster than contribution.

---

## 6. Pilot Business Recommendation

**Recommendation: PILOT, not SCALE.**

Move to broader rollout only when PrepLens demonstrates, with measured data:

* acceptable conversion from free preview to at least one paid path,
* positive contribution margin by paid workflow,
* controlled mock-interview cost per completed session,
* repeat usage or repeat purchase that supports sustainable acquisition economics,
* and acceptable quality / trust metrics from the evaluation framework.
