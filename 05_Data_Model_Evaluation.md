# Deliverable 05: Data, Model, and Evaluation Strategy

## 1. Problem Translated into AI Tasks

PrepLens does not rely on a monolithic black box; it decomposes the interview coaching problem into four modular, operationally credible sub-tasks:

```
[Candidate Input]
  Resume (PDF/Text) + Job Description (Text)
         |
         v
+----------------------------------------------------------------------------------+
| Task 1: Context Extraction & Entity Normalization (NER / NLP)                    |
| * Extract core tech stack, seniority indicators, and key project metrics.        |
| * Output: Cleaned JSON representation of candidate profile & job requirements.   |
+----------------------------------------------------------------------------------+
         |
         v
+----------------------------------------------------------------------------------+
| Task 2: Contextual Generation & Scaffolding (Generative LLM)                     |
| * Synthesize intersection of Resume & JD.                                        |
| * Generate: 5 Free Questions OR 25 Comprehensive Questions with Model Answers.    |
| * Output: Structured JSON conforming to strict UI schema.                        |
+----------------------------------------------------------------------------------+
         |
         v
+----------------------------------------------------------------------------------+
| Task 3: Acoustic Capture & Transcription (Speech-to-Text / STT)                  |
| * Capture user verbal answer via browser microphone in real-time.                |
| * Transcribe speech to text while tracking word-level timestamps.                |
+----------------------------------------------------------------------------------+
         |
         v
+----------------------------------------------------------------------------------+
| Task 4: Behavioral Speech Analytics (Deterministic NLP + Heuristics)             |
| * Calculate Words Per Minute (WPM) = (Total Words / Audio Duration in Minutes).  |
| * Regex matching for filler words ("um", "uh", "actually", "like").              |
| * Silence interval detection (> 3.0s pause flags).                               |
+----------------------------------------------------------------------------------+
```

---

## 2. Data Sources, Pipeline & Schema

### Data Inputs
1. **User-Generated Unstructured Text:** Candidate Resume text (extracted via `pdf-parse`) and pasted Job Description text.
2. **Internal System Prompts:** Domain-specific system instructions, rubrics for "Fresher" vs. "Experienced" question difficulty, and few-shot formatting examples.
3. **External Reference Data:** Industry skill taxonomy and standard STAR interview rubric templates.

### Data Preprocessing & Context Budgeting
To control costs and prevent context overflow, inputs undergo sanitization before entering the prompt pipeline:
* **Resume Truncation:** Strip out non-essential boilerplate (contact info, repetitive formatting) to cap resume context at **< 1,200 tokens**.
* **JD Normalization:** Strip legal/EEO boilerplates from the bottom of job postings, isolating the responsibilities and technical requirements to **< 800 tokens**.
* **Prompt Schema Enforcement:** All LLM outputs are enforced via JSON schema mode (`response_format: { type: "json_object" }`) to eliminate UI rendering failures.

---

## 3. Baseline Alternatives & Justification

| Solution Candidate | Pros | Cons | Why PrepLens Selected / Rejected |
| :--- | :--- | :--- | :--- |
| **Simple Rule-Based Keyword Matcher** | Extremely cheap, deterministic, zero latency | Cannot generate novel questions; unable to assess conceptual nuances in a project description. | **Rejected** as core generator; retained only for regex filler-word counting. |
| **Fine-Tuned Open-Source LLM (e.g., Llama 3 8B)** | Full data control, offline deployability, zero vendor lock-in | High upfront GPU infrastructure costs, deployment complexity, cold-start latency on serverless backends. | **Rejected for MVP**; unviable for a lean pilot. |
| **Commercial Foundation Model API** | Strong reasoning, large context windows, structured output support, and fast time-to-market. | Dependency on provider uptime, privacy terms, rate limits, and changing price/performance. | **Selected for MVP:** Provider/model is configurable and should be chosen through repeatable quality, latency, privacy, and cost benchmarks rather than hard-coded permanently. |

---

## 4. Evaluation Framework & Rigor

PrepLens evaluates system performance across three distinct tiers: **Technical Performance**, **Content Quality**, and **Business / Guardrail Integrity**.

| Metric Type | Specific Metric | Measurement Methodology | Target / SLA Threshold |
| :--- | :--- | :--- | :--- |
| **Technical** | **P95 Text Generation Latency** | Server-side API latency tracking from request initiation to complete JSON response | **< 3.5 seconds** (for 5 questions)<br>**< 7.0 seconds** (for 25 questions) |
| **Technical** | **Speech-to-Text Word Error Rate (WER)** | Benchmark test against a 100-sample audio dataset of Indian-accented technical terms | **WER < 8.0%** |
| **Technical** | **JSON Parsing Error Rate** | Automated server logging of malformed LLM responses | **< 0.5%** of all API calls |
| **Content Quality** | **JD-Resume Contextual Relevance** | LLM-as-a-Judge scoring (rubric scale 1–5 evaluating question specificity to submitted JD) | **Score >= 4.2 / 5.0** |
| **Content Quality** | **Hallucinated Tech Stack Rate** | Manual audit of generated questions referencing tools/skills not mentioned in JD or Resume | **< 2.0%** of generated questions |
| **Business / Trust** | **Free-to-Paid Conversion Rate** | Ratio of unique users generating free questions who proceed to purchase any paid package (Basic, Pro, or Mock) | **>= 3.0%** (Pilot target) |
| **Business / Trust** | **Model Answer Dissatisfaction Rate**| User "Report Question / Inaccurate" click rate in the UI | **< 4.0%** of rendered questions |

---

## 5. Offline Evaluation Harness: "LLM-as-a-Judge"

To prevent shipping degraded prompt updates to production, PrepLens employs an automated offline evaluation pipeline using a frozen test set of 30 diverse resumes and job descriptions:

```
[Candidate Test Pair (Resume + JD)]
                 |
                 v
      [Candidate LLM Prompt] ---> Generates Questions & Model Answers
                                                |
                                                v
                                  [Evaluator Model: independent benchmark LLM / human-reviewed sample]
                                  Assesses output against 3 criteria:
                                  1. Contextual Grounding (Is it grounded in the JD?)
                                  2. Seniority Calibration (Is it appropriate for Freshers?)
                                  3. Scaffolding Depth (Are pitfalls and approaches clear?)
                                                |
                                                v
                                  Pass / Fail Scorecard (Threshold: >= 85/100)
```

---

## 6. Go / No-Go Launch Thresholds

Before transitioning from internal alpha to public pilot, the product must satisfy all four **Go/No-Go Launch Gates**:
1. **JSON Reliability Gate:** Zero fatal rendering breaks across 100 consecutive automated test generations.
2. **Latency Gate:** P90 response latency under 4.0 seconds on standard 4G network simulations.
3. **Speech Legibility Gate:** Real-time speech transcription successfully captures technical jargon (e.g., "Kubernetes", "PostgreSQL", "asynchronous") with >90% precision.
4. **Economic Safety Gate:** Backend token and audio guardrails enforce that no single user session can trigger >₹12 in cumulative API costs.
