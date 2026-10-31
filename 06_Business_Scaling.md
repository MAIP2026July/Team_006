# Deliverable 06: Business, Economics, and Scaling View

## 1. Monetization Strategy & Pricing Architecture

PrepLens implements an **Event-Driven Micro-Transaction Model** designed specifically around the purchasing psychology of Indian college students and early-career job seekers:

```
[Top of Funnel: Free Tier]
* 0 Financial Friction
* User uploads Resume + JD
* Receives 5 Custom-Tailored Questions
        |
        |  "Experience the Personalization Hook"
        v
[Conversion Moment: ₹40 Micro-Transaction (~$0.48)]
* Instant Razorpay Checkout (UPI, GPay, PhonePe, Paytm, Cards)
* Zero subscription commitment (eliminates recurring billing anxiety)
* Delivers:
  1. 25-Question Comprehensive Guide (Approach + Ideal Model Answer + Pitfalls)
  2. 1 Interactive Voice-Enabled AI Mock Interview Session with Scorecard
  3. Formatted PDF / Offline Download
```

### Why Micro-Transactions Over Subscriptions?
1. **Low Friction for Students:** In India, university students exhibit high resistance to recurring SaaS subscriptions (e.g., ₹499/month), which require credit cards or complex UPI autopay mandates. A one-time ₹40 impulse purchase is lower than the cost of a canteen coffee or snack.
2. **Event-Driven Demand:** Interview preparation is episodic, not continuous. Candidates need intense preparation 2–3 days prior to an interview, making a monthly recurring subscription feel wasteful to the customer.

---

## 2. Granular Unit Economics & Contribution Margin

The viability of an AI-powered SaaS product depends entirely on maintaining a wide spread between the price charged and the underlying token/audio inference costs. 

Below is the detailed **Unit Economics Breakdown** for a single ₹40 paid user session:

| Cost / Revenue Component | Calculation Basis | Cost (INR) | % of Revenue |
| :--- | :--- | :--- | :--- |
| **Gross Revenue per Transaction** | Single Unlock Fee | **₹40.00** | **100.0%** |
| **Payment Gateway Fee (Razorpay)** | 2.0% transaction fee + 18% GST on fee (2.36% net) | -₹0.94 | 2.35% |
| **LLM Inference: Input Context** | ~2,000 prompt tokens (Resume + JD + System Prompt) @ $0.15 / 1M tokens | -₹0.02 | 0.05% |
| **LLM Inference: Output Generation** | ~4,500 output tokens (25 questions + model answers) @ $0.60 / 1M tokens | -₹0.23 | 0.58% |
| **Speech-to-Text Audio (STT)** | 8 minutes of audio transcription via AWS Speech-to-Text (Amazon Transcribe) | -₹4.03 | 10.08% |
| **Cloud Hosting & Database (Supabase)**| Serverless compute, egress bandwidth, and database write | -₹0.80 | 2.00% |
| **Customer Support & Buffer** | Dispute allocation and logging overhead | -₹0.50 | 1.25% |
| **Total Variable Cost of Goods Sold (COGS)**| **Sum of Variable Operational Costs** | **-₹6.52** | **16.3%** |
| **Net Contribution Margin per User** | **Revenue - Total COGS** | **₹33.48** | **83.7%** |

> **Key Insight:** Even after factoring in audio speech transcription and payment processing, PrepLens maintains an extraordinary **~83.7% gross contribution margin**.

---

## 3. Power-User Scaling Risks & Mathematical Quota Guardrails

### The "Runaway Audio Cost" Threat
While text inference is virtually costless (< ₹0.30 per generation), audio transcription is the single largest cost vulnerability. If an unconstrained user practices in the mock interview room for 45 minutes:
$$\text{45 minutes} \times \$0.006/\text{minute} \times 84\text{ INR/\$} = \mathbf{₹22.68\text{ STT Cost}}$$
Adding gateway and LLM costs would push total COGS to ~₹25.00, eroding the gross margin from 83% down to 37%.

### The Architectural Guardrail
To safeguard unit economics, PrepLens enforces **session-level throttling constraints**:
* Each ₹40 unlock entitles the user to **1 Mock Interview Pass** consisting of:
  * Maximum **5 simulated voice questions**.
  * Hard audio recording cap of **2 minutes per response** (Total session cap: 10 minutes maximum audio).
  * Enforces an absolute ceiling on STT cost at **₹5.04**, guaranteeing a minimum contribution margin of **> 75%** even in worst-case usage scenarios.
* Additional mock sessions are monetized as add-on booster packs (e.g., *"3 Additional Mock Sessions for ₹49"*).

---

## 4. Customer Acquisition & LTV / CAC Dynamics

### Acquisition Channels (Low CAC Strategy)
* **Campus Placement Ambassadors:** Appoint student reps across Tier-2 and Tier-3 engineering colleges offering placement drive prep kits.
* **Direct Job-Post Hijacking (Organic Social):** Publish automated, highly tailored question guides on LinkedIn under trending hiring posts (e.g., *"Accenture is hiring 2,000 Associate Software Engineers—here are 5 tailored questions from PrepLens"*).
* **Target Blended CAC:** **₹10 – ₹15** per acquired user.

### Customer Lifetime Value (LTV) Projection
While a single purchase yields ₹40, the average candidate in placement season interviews for 2 to 4 companies over a 3-month cycle.
* Repeat Purchase Rate: Estimated at 35% based on multi-interview job search patterns.
* Average Transactions per Active User ($N$): 1.35 purchases.
* Gross Revenue per Customer: $1.35 \times ₹40 = \mathbf{₹54.00}$.
* Net Contribution Profit per Customer: $1.35 \times ₹33.48 = \mathbf{₹45.20}$.
* **LTV to CAC Ratio:**
  $$\frac{\text{Net LTV}}{\text{CAC}} = \frac{₹45.20}{₹15.00} = \mathbf{3.01\times}$$
  *(A 3:1 ratio satisfies standard venture and MBA benchmarks for healthy SaaS unit economics).*

---

## 5. Scaling Constraints & Operational Bottlenecks

1. **Provider Rate Limiting During Peak Campus Hours:**
   * During hiring assessment days (typically Saturday/Sunday mornings), thousands of students prepare simultaneously. To prevent hitting LLM API tier concurrency caps, PrepLens implements an asynchronous queuing system with visual progress indicators.
2. **Payment Gateway Latency & Drop-offs:**
   * UPI failure rates on 4G campus networks can hit 8–12%. PrepLens integrates instant abandoned-cart retries and fallback UPI QR codes directly on the web interface.
3. **Domain Coverage Drift:**
   * Expanding from IT/Software to non-tech fields (Sales, Finance, Operations) requires tailored system prompt libraries to prevent generic, low-value model answers.
