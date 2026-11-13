# Deliverable 03: PM Artifact Pack & PRD-Lite

## 1. Problem Statement
Job seekers—particularly fresh graduates and early-career developers—often prepare with generic interview content that is disconnected from the specific role they are targeting. PrepLens treats the scale and causes of this preparation gap as **pilot hypotheses to validate**, rather than asserting a universal interview-failure rate.

Existing prep tools provide static, one-size-fits-all questionnaires that fail to evaluate a candidate’s unique project experience against real-world job criteria. Moreover, candidates lack affordable, objective environments to practice speaking under pressure, leading to speech dysfluency, rambling, and poor interview performance.

---

## 2. Target User Personas

### Persona A: "Arjun" – Final-Year B.Tech Student (Primary)
* **Demographics:** 21 years old, Computer Science student at a Tier-2 engineering college in India.
* **Context:** Campus placement season is underway. He has 3 interviews scheduled in two weeks for varying roles (Full-stack developer at a fintech startup, QA Engineer at an IT consultancy, Python developer at an enterprise).
* **Pain Point:** Has built generic college projects (e.g., e-commerce clone), but does not know what specific questions the fintech startup will ask about his projects. Cannot afford ₹3,000 for a private coaching session.
* **Goal:** Wants an instant, reliable list of likely interview questions matching his resume to the fintech JD, along with sample answers and a safe space to practice out loud.

### Persona B: "Priya" – Early-Career Career Switcher (Secondary)
* **Demographics:** 24 years old, 1.5 years experience in IT support, looking to transition into a Junior React/Node.js Developer role.
* **Pain Point:** Suffers from severe interview anxiety. During interviews, her speech accelerates to >180 WPM with frequent filler words ("um", "like", "you know"), causing interviewers to doubt her technical confidence.
* **Goal:** Wants objective feedback on her verbal delivery and pacing, coupled with structured answers explaining how her past projects map to junior developer responsibilities.

---

## 3. Product Requirements Document (PRD-Lite)

### 3.1 Functional Requirements (FR)

#### FR-1: Candidate Profile & Context Ingestion
* **Description:** The system must capture structured and unstructured candidate context to condition downstream AI prompts.
* **User Story:** *As a job seeker, I want to input my target industry, career stage, target job description, and upload my resume so the AI understands my exact professional profile.*
* **Specs & Validation:**
  * Support file uploads in `.pdf` and `.docx` format (max file size: 5 MB).
  * System extracts plain text using server-side document parsing libraries (e.g., `pdf-parse`).
  * If resume upload is omitted, fallback to manual text entry of key skills.
  * Input validation: JD must contain a minimum of 50 characters to prevent low-entropy prompt hallucinations.

#### FR-2: Freemium Contextual Question Generation (The Hook)
* **Description:** Generates an immediate teaser set of 5 tailored interview questions.
* **User Story:** *As a user, I want to see a preview of personalized questions immediately so I can verify that PrepLens understands my unique resume.*
* **Specs & Validation:**
  * System prompts the LLM with extracted Resume tokens + JD tokens + Stage ("Fresher" vs "Experienced").
  * System renders a responsive, full-page loading state to maintain user engagement during the 3–5 second generation window.
  * Outputs 5 categorized questions (3 Technical, 1 Project-Specific, 1 Behavioral).

#### FR-3: Monetization & Paywall Integration (Razorpay)
* **Description:** Micro-transaction gateway gating the Comprehensive Prep Guide and Voice Room.
* **User Story:** *As a candidate, I want to choose an affordable one-time preparation package—Basic (₹30), Pro (₹60), or AI Mock Interview (₹100)—so I can pay only for the depth of preparation I need.*
* **Specs & Validation:**
  * Integrated with Razorpay Standard Checkout API.
  * User submits Full Name and Email Address.
  * On successful webhook payment verification (`order_paid`), backend issues an encrypted unlock token granting access to the 25-Question Guide.

#### FR-4: Comprehensive Prep Guide & Model Answers
* **Description:** Full-scope preparation guide with structured answer scaffolding.
* **User Story:** *As a paid user, I want to see detailed model answers and common pitfalls for 25 questions so that I know exactly how to structure my responses.*
* **Specs & Validation:**
  * Delivers 25 questions grouped into:
    * *Technical Core (10 questions)*
    * *Project Deep-Dive (8 questions)*
    * *Behavioral / Scenario-Based (7 questions)*
  * Each question must display a 3-part accordion structure:
    1. **Strategic Approach:** How to frame the answer (e.g., using the STAR technique).
    2. **Ideal Model Answer:** Contextualized to the user’s tech stack and past projects.
    3. **Common Pitfalls:** Exact mistakes that lead to candidate rejection.
  * Action: Download guide offline as formatted text or PDF.

#### FR-5: Voice-Enabled AI Mock Interview Room
* **Description:** Simulated interview room with audio transcription and pacing analytics.
* **User Story:** *As a candidate, I want to verbally answer interview questions and receive instant speech feedback so that I can practice under realistic interview conditions.*
* **Specs & Validation:**
  * Utilizes browser Web Speech API / server-side Speech-to-Text.
  * Microphone input captured in real-time; displays live audio waveform visualization.
  * Displays one question at a time with a countdown timer (e.g., 2 minutes per answer).
  * Candidate audio is transcribed and saved to session state.

#### FR-6: Performance Analytics & Scorecard
* **Description:** Post-interview analytical dashboard evaluating speech and content.
* **User Story:** *As a candidate, I want to review my speech metrics after the mock interview so that I can identify weak delivery habits.*
* **Specs & Validation:**
  * **Words Per Minute (WPM):** Calculates speech rate (Optimal benchmark: 120–150 WPM). Flags <100 WPM as "too slow/hesitant" and >160 WPM as "rushed".
  * **Filler Word Counter:** Flags instances of "um", "uh", "like", "actually", "basically".
  * **Pause & Hesitation Analysis:** Flags prolonged silences (>3 seconds).

---

### 3.2 Edge Cases and Fallback Logic

| Scenario / Failure Mode | System Behavior & Fallback Mechanism |
| :--- | :--- |
| **Scanned Image-Only PDF Resume** | Parser detects zero extractable text string characters. System displays a polite non-blocking alert: *"We couldn't read text in this PDF. Please upload a standard text PDF or paste your skills manually."* |
| **Vague / Empty Job Description** | If JD is < 50 characters, system disables the generation CTA and prompts: *"Please provide at least a brief job summary so questions are accurately tailored."* |
| **Third-Party LLM API Outage / Timeout** | If LLM API fails to respond within 8 seconds, system catches error, retries once with backoff, and if still down, falls back to a curated domain-specific question template bank while refunding/preserving the user's credits. |
| **Microphone Permission Denied** | If browser microphone access is blocked by the candidate, system displays an in-app banner with permission instructions and provides a "Switch to Text-Chat Mode" toggle. |
| **Payment Abandonment / Failure** | Razorpay drops out or user cancels. Backend records an `abandoned_checkout` analytics event. User remains on the 5-question page with an inline coupon discount banner (e.g., "Use code PREP20 for 20% off"). |

---

## 4. Feature Prioritization Framework (RICE Analysis)

Features were scored using the **RICE** methodology (Reach × Impact × Confidence / Effort) to determine MVP inclusion:

| Feature Candidate | Reach (Monthly Users) | Impact (0.5–3) | Confidence (%) | Effort (Person-Weeks) | RICE Score | Decision |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Resume × JD 5-Question Teaser** | 10,000 | 2.0 (High) | 90% | 2 | **9,000** | **Must Have (MVP)** |
| **Paid Prep Guides (Basic ₹30 / Pro ₹60)** | 4,000 | 3.0 (Massive) | 85% | 2 | **5,100** | **Must Have (MVP)** |
| **Voice Mock Interview (Browser STT)** | 2,500 | 2.5 (High) | 70% | 3 | **1,458** | **Core Wedge (MVP)** |
| **Speech Analytics Scorecard (WPM/Fillers)** | 2,000 | 1.5 (Med) | 80% | 2 | **1,200** | **Phase 2 (Immediate)**|
| **Export Prep Guide to PDF** | 3,000 | 1.0 (Low) | 90% | 1 | **2,700** | **Must Have (MVP)** |
| **Live Multi-turn AI Video Avatar** | 1,000 | 2.0 (High) | 40% | 8 | **100** | **Deprioritize (Post-Pilot)** |

---

## 5. Phased Product Roadmap

```
+---------------------------------------------------------------------------------------+
| PHASE 1: Controlled MVP (Weeks 1-4)                                                  |
| * Resume & JD text ingestion                                                          |
| * Single-shot generation of 5 free questions                                         |
| * Razorpay payment integration for Basic ₹30 / Pro ₹60 / Mock ₹100                  |
| * Delivery of 25-question Prep Guide with Model Answers & PDF export                   |
| Gate: Verify > 3.0% free-to-paid conversion rate across 200 organic users.             |
+---------------------------------------------------------------------------------------+
                                           |
                                           v
+---------------------------------------------------------------------------------------+
| PHASE 2: Interactive Voice Mock Room (Weeks 5-8)                                      |
| * Browser-based Speech-to-Text audio capture                                         |
| * Timed question-answer cycle with simulated interviewer prompts                      |
| * WPM and filler-word analytics scorecard                                             |
| Gate: Mock interview completion rate > 60%; Voice transcription accuracy > 92%.        |
+---------------------------------------------------------------------------------------+
                                           |
                                           v
+---------------------------------------------------------------------------------------+
| PHASE 3: Domain Specialization & Placement Kits (Weeks 9-14)                          |
| * Domain-tailored survival kits (Data Science, Cloud/DevOps, Core Engineering)        |
| * Multi-turn follow-up questioning based on candidate verbal answers                  |
| * Referral & college ambassador discount program                                      |
| Gate: 30-day retention / repeat interview purchase rate > 25%.                         |
+---------------------------------------------------------------------------------------+
                                           |
                                           v
+---------------------------------------------------------------------------------------+
| PHASE 4: B2B University Placement Cell Integration (Long Term)                       |
| * Campus batch dashboard for Training & Placement Officers (TPOs)                     |
| * Cohort-level readiness benchmarking analytics                                       |
+---------------------------------------------------------------------------------------+
```

---

## 6. Success Metrics Hierarchy

```
                                  [North Star Metric]
                          Weekly Successful Interview Preps 
                      (Users who generate guide & practice >= 3 questions)
                                           |
            +------------------------------+------------------------------+
            |                                                             |
   [Product Engagement]                                          [Business & Monetization]
- Free-to-Paid Conversion Rate (Pilot target: 3.5%; to be validated)                - Gross Contribution Margin (Target: > 70%)
- Mock Session Completion Rate (Target: > 65%)               - Cost of Goods Sold (COGS) per User (Target: < ₹10)
- Average Practice Time per Session (> 8 mins)               - CAC Payback (< 3 days; pilot hypothesis, not observed performance)
            |                                                             |
            +------------------------------+------------------------------+
                                           |
                              [Trust & Guardrail Metrics]
                   - LLM Generation Latency (P95 < 4.0s)
                   - Speech-to-Text Word Error Rate (WER < 8%)
                   - User Model Answer Dissatisfaction Rate (< 5% negative flags)
```
