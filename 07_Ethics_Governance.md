# Deliverable 07: Ethics, Governance, and Risk Note

## 1. Ethical Vulnerabilities & Threat Modeling

As an AI-powered assessment tool evaluating human career readiness, PrepLens operates in an ethically sensitive domain where algorithmic errors carry tangible real-world consequences for job seekers.

```
+---------------------------------------------------------------------------------------+
|                                ETHICAL RISK TAXONOMY                                  |
+---------------------------------------------------------------------------------------+
|  1. Data Privacy & PII     |  2. Accent & Acoustic Bias |  3. Over-Reliance / Hallucination|
|  * Exposure of resumes     |  * Non-standard accents    |  * Candidates memorizing         |
|  * Unconsented AI training |  * Acoustic discrimination |    factually incorrect answers   |
|  * DPDPA 2023 compliance   |  * Neurodivergent pacing   |  * Unrealistic expectations      |
+---------------------------------------------------------------------------------------+
```

---

## 2. Deep-Dive on Core Risks & Harm Scenarios

### Risk 1: Personal Data Exposure & Foundation Model Ingestion
* **The Vulnerability:** Resumes are dense repositories of Personally Identifiable Information (PII)—including candidate full names, phone numbers, personal email addresses, home locations, educational transcripts, and previous employer data.
* **Harm Scenario:** If resume text is sent to public, non-enterprise LLM endpoints that utilize prompt data for model retraining, candidate personal information could be leaked into foundation model weights or third-party query logs, violating the **Digital Personal Data Protection Act (DPDPA 2023)** of India.
* **Governance Control:** 
  1. *Enterprise Zero-Data Retention (ZDR):* All backend API integrations (OpenAI / Google Gemini) utilize commercial enterprise endpoints governed by explicit ZDR agreements, ensuring user prompts are processed in-memory and discarded immediately without logging for model training.
  2. *Client-Side PII Masking:* Before text is serialized into the generation payload, regex filters mask phone numbers and street addresses, preserving only professional experience and technical skills.

### Risk 2: Acoustic and Accent Bias in Speech Recognition
* **The Vulnerability:** Commercial Speech-to-Text (STT) models have historically exhibited higher Word Error Rates (WER) for non-native English speakers and regional Indian accents (e.g., Tamil, Telugu, Hindi, Bengali inflections) compared to standard Western accents.
* **Harm Scenario:** An engineering student with strong technical competence and a heavy regional accent enters the AI Mock Interview room. The STT engine misinterprets their answers, transcribes technical terms incorrectly (e.g., transcribing "Kubernetes" as "copper net is"), and the analytics scorecard flags their performance as incoherent. This crushes candidate morale and misleads their interview preparation.
* **Governance Control:**
  1. *Phonetic Technical Prompting:* We feed a context dictionary of 500+ common Indian IT technical terms into the transcription prompt/decoder to bias transcription accuracy toward domain jargon.
  2. *Decoupling Speech Delivery from Content Assessment:* The scorecard explicitly isolates communication metrics (WPM, pauses) from the technical model answer comparison. The UI displays an unmissable disclaimer: *"Speech analytics reflect acoustic pacing and delivery rhythm; they do not measure your intelligence, accent validity, or technical capability."*

### Risk 3: Hallucination and Over-Reliance ("The Parrot Trap")
* **The Vulnerability:** Large Language Models can generate convincingly worded answers containing subtle architectural errors or outdated syntax.
* **Harm Scenario:** A candidate blindly memorizes an AI-generated model answer containing a deprecated library method. When the interviewer probes the answer in a live interview, the candidate is unable to explain the underlying logic and is rejected for superficial knowledge.
* **Governance Control:**
  1. *Structural Framing Over Scripts:* The Prep Guide never labels text as *"The Exact Answer to Say"*. Instead, it frames answers as:
     * **Strategy / Approach:** How to think about the problem.
     * **Model Answer Concept:** Key architectural points to highlight.
     * **Common Traps:** Warnings about where candidates often stumble.
  2. *Mandatory "Check & Customize" Prompt:* The UI encourages users to adapt answers into their own voice before attending interviews.

---

## 3. Governance Register & Mitigation Architecture

| Domain | Risk Description | Severity | Likelihood | Enforced Mitigation Control |
| :--- | :--- | :--- | :--- | :--- |
| **Privacy** | Unauthorized storage of candidate resumes | High | Low | Ephemeral processing: resumes are converted to text in volatile memory and purged post-generation. No permanent PDF storage on disk. |
| **Fairness** | Penalizing neurodivergent speech patterns (stuttering, slow cadence) | Medium | Medium | WPM benchmark can be customized in user settings or completely disabled; non-evaluative coaching tone used in feedback. |
| **Safety** | Malicious prompt injection via uploaded JD | Low | Medium | Strict sanitization of user input strings; stripping system delimiter tags (`<|im_start|>`, etc.) prior to prompt compilation. |
| **Consumer Protection** | Payment charged but generation failed | High | Low | Automated webhook retry queue in Supabase; auto-refund initiated if generation cannot complete within 3 minutes. |

---

## 4. Hard Go/No-Go Launch Blockers

The product will **immediately halt or pause deployment** if any of the following triggers occur:
1. **The PII Breach Blocker:** Any confirmation that API providers are retaining or training on candidate resume text without authorization.
2. **The Accent Disparity Blocker:** If real-world beta testing reveals a Word Error Rate (WER) disparity of greater than **10%** between standard English and regional Indian-accented English speech samples.
3. **The Hallucination Cascade Blocker:** If automated evaluation checks flag factually incorrect or toxic interview advice in more than **3%** of generated prep guides.
