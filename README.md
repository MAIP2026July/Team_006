# PrepLens AI: Management of AI Products Capstone (MBA ZG583)

## 📌 Student \& Submission Metadata

* **Student Name:** Yuvaraja Kannan
* **Roll Number:** `2025mb26082`
* **GitHub ID:** `yuvaraja77`
* **Course:** MBA ZG583 — Management of AI Products (S1-26\_MBAZG583)
* **Team Name:** PrepLens Labs (Approved Solo Project)
* **Live Product Demo:** [https://preplensai.app](https://preplensai.app) 
* **Repository:** `https://github.com/MAIP2026July/Team\_006.git`

\---

## 🚀 1-Minute Executive Summary 

**PrepLens AI** is an intelligent, domain-specific interview coach engineered to eliminate candidate interview failure caused by **preparation-context mismatch**.

* **The Problem:** Technical job seekers (specifically engineering freshers) face a 70–90% rejection rate in interviews. Failure rarely stems from coding inability; it stems from candidates preparing for generic textbook questions rather than the exact tech stack and operational context specified in the employer’s **Job Description (JD)**.
* **The Solution:** PrepLens synthesizes an unstructured candidate resume alongside the target JD to generate hyper-personalized questions, structured model answers, and a live voice-simulated mock interview room with real-time speech analytics.
* **The Unit Economics:** An event-driven micro-transaction model (**₹40 per unlock**). With an inference + STT cost of **₹6.52**, PrepLens delivers an extraordinary **83.7% Gross Contribution Margin** per transaction.

\---

## 🗂️ Complete Product Management Artifact Index

As instructed for the capstone submission, all artifacts are documented in unambiguous, AI-evaluable Markdown (`.md`) format:

|Artifact File|Description \& Core PM Value|
|-|-|
|[**`PRD.md`**](./PRD.md)|**Primary Mandatory Artifact:** Comprehensive Product Requirements Document containing user personas, 6 functional specifications, non-functional latency/security SLAs, failure edge cases, and Go/No-Go release criteria.|
|[**`01\_Executive\_Summary.md`**](./01_Executive_Summary.md)|2-page leadership briefing synthesizing problem, market size, AI justification, economics, and pilot recommendation.|
|[**`02\_Product\_Strategy.md`**](./02_Product_Strategy.md)|Product vision, JTBD (functional, emotional, social), RICE opportunity matrix, and **Course Case Study Benchmarking (OpenAI Pricing \& Yellow.ai)**.|
|[**`03\_PM\_Artifact\_Pack.md`**](./03_PM_Artifact_Pack.md)|Detailed Persona profiles ("Arjun" the fresher, "Priya" the career switcher), phased 4-version roadmap, and quantitative metric trees.|
|[**`04\_UX\_and\_Workflow.md`**](./04_UX_and_Workflow.md)|**Mermaid Swimlane Workflow Diagram** classifying deterministic vs. probabilistic steps, hallucination risk points, and Human-in-the-Loop review gates.|
|[**`05\_Data\_Model\_Evaluation.md`**](./05_Data_Model_Evaluation.md)|AI task decomposition, context token budgeting (<2,000 tokens), **LLM-as-a-Judge offline evaluation harness**, and quantitative SLA thresholds.|
|[**`06\_Business\_Scaling.md`**](./06_Business_Scaling.md)|Target costing analysis, granular unit economics table (₹40 revenue vs ₹6.52 COGS), runaway audio quota guardrails, and LTV:CAC model (3.01x).|
|[**`07\_Ethics\_Governance.md`**](./07_Ethics_Governance.md)|DPDPA 2023 compliance, Zero-Data Retention (ZDR) enterprise policies, regional Indian accent bias mitigations in STT, and launch blockers.|
|[**`08\_Prototype\_Readme.md`**](./08_Prototype_Readme.md)|Prototype documentation outlining what the live application proves vs. what is deferred.|
|[**`09\_AI\_Collaboration\_Log.md`**](./09_AI_Collaboration_Log.md)|Transparent disclosure of AI tool usage, prompt evolution, AI weaknesses, and human PM strategic overrides.|
|[**`10\_Individual\_Contribution.md`**](./10_Individual_Contribution.md)|Solo contributor disclosure documenting artifact ownership and validation rigor.|

\---

## 🏗️ System Architecture \& Workflow Diagram

```mermaid
graph TD
    subgraph Candidate\_Inputs\["1. Candidate Input Layer"]
        Resume\["Upload Resume (PDF/Docx)"]
        JD\["Paste Target Job Description (Text)"]
        Stage\["Select Experience Stage (Fresher / Experienced)"]
    end

    subgraph Deterministic\_Processing\["2. Deterministic Pipeline \[Zero AI Hallucination Risk]"]
        Sanitize\["Input Sanitization \& Length Validation (> 50 chars)"]
        PDFParser\["PDF Text Extraction (pdf-parse)"]
        TokenBudget\["Context Token Truncation (< 2,000 tokens)"]
    end

    subgraph Probabilistic\_AI\["3. Generative AI Engine \[Probabilistic - Guardrailed]"]
        PromptEngine\["Role-Conditioned System Prompt"]
        LLM\["Google Gemini 1.5 Flash / OpenAI GPT-4o-mini"]
        Questions5\["5-Question Free Teaser"]
        PrepGuide25\["25-Question Guide + Model Answers (STAR)"]
    end

    subgraph Monetization\["4. Micro-Transaction Gateway"]
        Razorpay\["Razorpay Gateway (₹40 UPI / Cards)"]
        HMAC\["Server-Side HMAC Signature Verification"]
    end

    subgraph Voice\_Simulation\["5. Interactive Voice Mock Room"]
        AudioRecord\["Browser Audio Capture (Web Speech API)"]
        STT\["Speech-to-Text Transcription"]
        SpeechAnalytics\["Deterministic Analytics (WPM, Fillers, Pauses)"]
    end

    Resume \& JD \& Stage --> Sanitize --> PDFParser --> TokenBudget
    TokenBudget --> PromptEngine --> LLM --> Questions5
    Questions5 --> Razorpay --> HMAC --> PrepGuide25
    PrepGuide25 --> AudioRecord --> STT --> SpeechAnalytics
```

\---

## 🎯 Verification \& Testing

The prototype includes automated test suites covering support handling, coupon validation, logic verification, and feedback logging:

```bash
cd backend
npm test
```

