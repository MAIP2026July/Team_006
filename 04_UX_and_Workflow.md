# Deliverable 04: UX, Workflow, and Trust Design

## 1. UX Principle & Human-AI Interaction Philosophy
PrepLens AI is explicitly designed as an **augmented interview coach**, not an autonomous evaluator. The interaction model treats Generative AI as a probabilistic drafting engine, ensuring that the **Candidate remains the ultimate decision-maker** who critiques, edits, and adopts the model answers.

---

## 2. Mermaid Swimlane Workflow Diagram

This diagram visually delineates the boundaries between the Candidate, the Web Interface, the Payment Gateway, and the AI/LLM backend, explicitly identifying **Deterministic** processes, **Probabilistic** AI stages, **Hallucination Risk points**, and **Human-in-the-Loop (HITL)** controls:

```mermaid
graph TD
    %% Lane 1: Candidate Actions
    subgraph Lane_Candidate["👤 LANE 1: CANDIDATE (Engineering Student)"]
        A1["1. Upload Resume (PDF) & Paste JD"] --> A2["2. Review 5-Question Free Preview"]
        A2 --> A3["3. Click 'Unlock Prep Guide' (₹40)"]
        A3 --> A4["4. Review 25-Question Guide & STAR Answers<br><b>[Human-in-the-Loop Override]</b>"]
        A4 --> A5["5. Speak Answers into Mic in Voice Room"]
        A5 --> A6["6. Review Delivery Scorecard & Download PDF"]
    end

    %% Lane 2: Frontend UI
    subgraph Lane_Frontend["💻 LANE 2: PREPLENS FRONTEND (Web UI)"]
        B1["Client-side Regex PII Scrubbing<br><b>[Deterministic]</b>"]
        B2["Full-Page Animated Progress Loader"]
        B3["Launch Razorpay Standard Modal"]
        B4["Render 25-Question Accordion UI"]
        B5["Web Audio Stream & Visual Waveform<br><b>[Deterministic]</b>"]
        B6["Display WPM, Fillers & Silence Diagnostics"]
    end

    %% Lane 3: Gateway
    subgraph Lane_Payment["💳 LANE 3: RAZORPAY GATEWAY"]
        C1["Process UPI / Card / NetBanking (₹40.00)"]
        C2["Emit 'order_paid' Webhook<br><b>[Deterministic HMAC Check]</b>"]
    end

    %% Lane 4: AI & Backend Engine
    subgraph Lane_Backend_AI["🧠 LANE 4: BACKEND & AI ENGINE (GPT-5 mini / AWS Speech-to-Text)"]
        D1["pdf-parse Text Extraction + Context Cap (< 2,000 tokens)<br><b>[Deterministic]</b>"]
        D2["GPT-5 mini: Synthesize 5 Questions<br><b>[Probabilistic / LLM]</b><br><i>(Guardrail: Strict overlap with JD)</i>"]
        D3["GPT-5 mini: Generate 25 STAR Answers & Pitfalls<br><b>[Probabilistic / LLM]</b>"]
        D4["AWS Speech-to-Text: Audio-to-Text Transcription<br><b>[Probabilistic - Target WER &lt; 8%]</b>"]
        D5["Calculate WPM & Match Regex Fillers ('um', 'like')<br><b>[Deterministic]</b>"]
    end

    %% Cross-Lane Connections
    A1 --> B1 --> D1 --> D2 --> B2 --> A2
    A3 --> B3 --> C1 --> C2 --> D3 --> B4 --> A4
    A4 --> A5 --> B5 --> D4 --> D5 --> B6 --> A6

    %% Styling
    style Lane_Candidate fill:#e3f2fd,stroke:#1565c0,stroke-width:2px;
    style Lane_Frontend fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px;
    style Lane_Payment fill:#fff3e0,stroke:#e65100,stroke-width:2px;
    style Lane_Backend_AI fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px;
```

---

## 3. Trust, Transparency, and Explainability Architecture

To foster trust without creating blind over-reliance, the user interface adheres to strict explainability guidelines:

* **What the Candidate Needs to Know:** Model Answers are structural scaffolding (using the STAR framework), not literal scripts to be memorized word-for-word.
* **What Must Be Explicitly Visible:**
  * **Keyword Provenance Tags:** Each question displays the specific skill tag extracted from the JD or Resume (e.g., `Grounded in: React Context API + Project: E-Commerce Store`), proving the question was tailored and not hallucinated.
  * **Latency Micro-States:** When generating questions, the animated loader displays active micro-copy (e.g., *"Analyzing requirements..."* $\rightarrow$ *"Cross-referencing resume..."* $\rightarrow$ *"Formatting rubric..."*) so the candidate understands that complex synthesis is occurring.
* **What Must Be Hidden:** Raw API system instructions, internal token counts, and intermediate temperature parameters.

---

## 4. Probabilistic vs. Deterministic Uncertainty Handling

| Execution Type | System Stage | High-Confidence Behavior | Low-Confidence / Ambiguous Behavior | Failure / Fallback Behavior |
| :--- | :--- | :--- | :--- | :--- |
| **Deterministic** | Resume & JD Intake | Scanned PDF text extracted cleanly (>500 chars). | Text extracted is brief (<100 chars); UI displays non-blocking warning to provide more context. | Scanned image-only PDF yields 0 characters; UI prompts candidate to paste skills manually. |
| **Probabilistic** | Question Generation | Questions directly map to JD technical stack. | Candidate resume has zero overlap with target JD; system flags mismatch and generates baseline domain questions. | LLM API timeout (>8.0s); catches error, retries once, and falls back to curated domain template. |
| **Deterministic** | Payment Verification | Razorpay HMAC signature validates instantly. | Webhook delayed by network; UI polls order status for up to 30 seconds. | Payment fails or is cancelled; preserves 5-question state and displays 20% discount coupon (`PREP20`). |
| **Probabilistic** | Speech Recognition (STT) | Audio transcribed cleanly with WER < 5%. | Audio contains heavy background noise or thick accent; technical terms biased via phonetic IT dictionary. | Microphone blocked or audio capture drops; system prompts 1-click permission fix or offers text-chat mode. |
| **Deterministic** | Speech Analytics | WPM calculated accurately (110–150 WPM benchmark). | Short response (<15 seconds); flags sample as "Too brief to compute pacing metric accurately." | Zero speech detected; flags "No audio received, please verify microphone input." |

---

## 5. Human-in-the-Loop (HITL) Controls

The candidate is never a passive consumer; the workflow enforces three active review gates:
1. **The Customization Gate:** Candidates can edit any AI-generated model answer directly in the browser to reflect their actual personal speaking style before downloading.
2. **The Feedback Gate:** Every generated question includes a 1-click flag icon (*"Inaccurate Tech Concept"*), sending the query to an admin review queue to refine system prompts.
3. **The Practice Gate:** Candidates evaluate their own diagnostic scorecard (WPM and filler words) to decide whether to re-record an answer or move forward.
