# Deliverable 02: Product Strategy and Opportunity Framing

## 1. Executive Product Vision
**PrepLens AI** empowers job seekers to eliminate interview failure caused by preparation-context mismatch. By providing an intelligent, domain-specific AI coach that cross-examines a candidate's actual resume against specific job descriptions (JDs), PrepLens delivers hyper-personalized interview questions, structured model answers, and voice-simulated mock interview pressure with speech analytics.

---

## 2. Jobs To Be Done (JTBD) Framework

### Primary Job
* **When I am** an engineering student or early-career professional preparing for a high-stakes job interview,
* **I want to** practice answering realistic, technically rigorous questions tailored specifically to the intersection of my resume and the employer’s job description,
* **So that I can** articulate my real-world skills fluently, avoid freeze-ups or generic rehearsed answers, and significantly improve my offer conversion rate.

### Supporting / Emotional & Social Jobs
* **Functional:** Identify the blind spots and skill gaps between my current resume and what the target employer demands before the interviewer catches them.
* **Emotional:** Overcome imposter syndrome and interview anxiety by experiencing the acoustic and cognitive pressure of answering aloud in a simulated environment.
* **Social:** Signal competence and professionalism to hiring panels by framing past projects in the exact vocabulary and expectations of the target company.

---

## 3. Opportunity Framing & Problem Breakdown

### Why This Problem Matters
In today's hyper-competitive tech recruitment landscape, hundreds of applicants contest for single openings. Technical candidates may spend substantial time learning data structures, frameworks, and syntax, yet still struggle to convert that preparation into strong interview performance. For this capstone, PrepLens treats the following as **product hypotheses to validate in pilot research**, rather than established causal facts:
1. **Preparing for the Resume instead of the JD:** Candidates rehearse generic summaries of their past projects without mapping their experience to the company's specific stack, architectural patterns, and business domain.
2. **The "Passive Reading" Trap:** Reading blogs or static lists creates an illusion of competence. When put under live verbal pressure, candidates stumble, use excessive filler words, and fail to structure answers concisely.
3. **Cost and Scheduling Friction:** Human mock-interview coaching can be expensive and difficult to schedule for repeated practice. PrepLens therefore tests whether a low-cost, self-serve alternative can satisfy the preparation need for price-sensitive candidates.

### Why Existing Alternatives Fail
| Alternative | Mechanism | Fatal Flaw |
| :--- | :--- | :--- |
| **Static Question Portals** (GeeksforGeeks, LeetCode, Glassdoor) | Generic lists of "Top 50 React Questions" | Zero personalization; fails to synthesize the candidate's unique project scope with the specific JD requirements. |
| **Raw ChatGPT / LLM Prompting** | User manually writes prompts like *"Ask me interview questions for this JD"* | High cognitive friction; requires expert prompt engineering to extract structured rubric-based answers; lacks audio simulation, pacing analytics, and payment-gated preparation guides. |
| **Peer / Mirror Practice** | Practicing out loud alone or with college peers | Zero objective feedback; no domain expertise to evaluate whether the answer met industry benchmarks. |
| **Human 1-on-1 Mentorship** | Booking verified tech mentors | Scheduling friction and a higher cost per practice session than self-serve digital preparation. |

---

## 4. Opportunity Matrix & Prioritization

To avoid the trap of "automating everything," potential AI features are evaluated against business value and technical feasibility:

| AI Feature / Capability | Business Value | Technical Feasibility | Strategic Priority | Rationale / Placement |
| :--- | :--- | :--- | :--- | :--- |
| **Resume × JD Contextual Question Generation** | **High** | **High** | **P0 (Core Wedge / MVP)** | Solves the primary pain point with fast inference and high margin; acts as the primary hook. |
| **Comprehensive Prep Guide with Model Answers** | **High** | **High** | **P0 (Monetization Engine)** | Structured output (Approach + Ideal Answer + Pitfalls to avoid); supports conversion into the paid **Basic (₹30)** or **Pro (₹60)** preparation guide. |
| **Browser-Based Voice Mock Interview (STT)** | **High** | **Medium** | **P1 (Core Differentiator)** | Bridges the gap between passive reading and active verbal articulation; creates the "wow" factor. |
| **Speech Analytics (WPM, Filler Words, Pacing)** | **Medium** | **High** | **P1 (Feedback Layer)** | Objective rule-based + NLP metrics derived from transcription timestamps; reinforces user trust. |
| **Live Dynamic Multi-Turn Follow-Up Agent** | **High** | **Low** | **P2 (Phase 3 Roadmap)** | High latency and context management risk for MVP; deferred until single-turn simulation is proven. |
| **Automated Code Execution & Sandbox** | **Medium** | **Low** | **P3 (Deprioritized)** | LeetCode/HackerRank already dominate coding tests; PrepLens focuses on behavioral, design, and domain discussions. |

---

## 5. Architectural Decision: Build vs. Buy vs. Partner/API

For the MVP and pilot scaling, PrepLens selects an **API-First & Workflow Orchestration Architecture**:

* **Text Generation (LLM):** Partner via a commercial foundation-model API, with the provider/model selected through quality, latency, structured-output reliability, privacy terms, and cost benchmarks.
  * *Why:* Training a proprietary foundation model is unnecessary for the MVP. An API-first approach maximizes learning speed and lets PrepLens switch models as economics and performance change.
* **Audio & Speech-to-Text (STT):** AWS Speech-to-Text (Amazon Transcribe).
  * *Why:* Low-latency transcription and enterprise-grade acoustic modeling. AWS Transcribe provides robust recognition of diverse technical jargon and regional speech patterns.
* **Proprietary Moat / What We Build:**
  * Proprietary prompt engineering pipeline with role-conditioned system rubrics.
  * Context parsing algorithms extracting hard tech skills, project constraints, and seniority tiers.
  * Tailored scoring heuristics for communication speed (WPM) and filler word frequency.
  * End-to-end user experience, Razorpay checkout flow, and downloadable prep guide compilation.

---

## 6. Course Case Study Integration & Benchmarking

To ground the product strategy in proven industry patterns, PrepLens benchmarks against two core cases from the MBA curriculum:

### Case 1: OpenAI Pricing, API Economics & Platform Dynamics
* **What is Similar:** PrepLens is a vertical wrapper operating on top of commodity foundation models. Like startups analyzed in the OpenAI ecosystem, our Cost of Goods Sold (COGS) is tied directly to API token usage and audio transcription duration.
* **What is Different:** Foundation-model providers sell horizontal compute through usage-based APIs. PrepLens decouples backend cost from user pricing through simple one-time product packages: **Basic ₹30, Pro ₹60, and AI Mock Interview ₹100**.
* **Lessons Transferred:** 
  1. *Token Inflation Vulnerability:* Long system prompts and uncontrolled user inputs can rapidly inflate inference costs. PrepLens applies strict pre-processing (truncating redundant resume boilerplate) to cap prompt token expenditure at < 2,500 tokens per call.
  2. *Session Guardrails:* Usage-based AI economics demonstrate why PrepLens must impose session caps (e.g., maximum 10 minutes or 5 voice questions per mock interview pass) to prevent power users from consuming margin.
* **What PrepLens Does Differently:** Instead of an opaque subscription that induces consumer churn, PrepLens leverages an event-driven micro-transaction model tailored to the Indian student demographic.

### Case 2: Yellow.ai & Enterprise Conversational UX
* **What is Similar:** Both products manage human-AI conversational loops where perceived latency and dialogue accuracy dictate user trust.
* **What is Different:** Yellow.ai builds multi-turn enterprise customer service bots where failure leads to customer churn; PrepLens builds a training simulation where the candidate expects challenging friction and evaluative feedback.
* **Lessons Transferred:**
  1. *Graceful Latency Masking:* Yellow.ai uses immediate visual micro-states ("AI is evaluating your response...") to prevent abandonment during model inference. PrepLens implemented a full-page animated loading state and streaming cues.
  2. *Hybrid Rule/Probabilistic Architecture:* Yellow.ai combines rigid business rules with generative flexibility. PrepLens adopts this: speech analytics (WPM, pause count) use deterministic rule-based algorithms, while model answers use generative LLMs.
* **What PrepLens Does Differently:** Avoids open-ended multi-turn rambling. PrepLens structures mock sessions into clear discrete stages (Question → Candidate Audio → Real-time Transcript → Model Comparison → Scorecard), ensuring predictable UX boundaries.

---

## 7. The First Wedge (Target Market Focus)
* **Target Segment:** Final-year B.Tech / BCA / MCA engineering students and early-career tech professionals (0–2 years experience) in India.
* **Initial Domain:** IT, Web Development, and Core Software Engineering roles.
* **Go-To-Market Trigger:** Campus placement seasons (August–November) and off-campus hiring drives, where anxiety is highest and willingness-to-pay for fast, reliable prep is peak.
