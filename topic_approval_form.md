## Topic Approval Form

### Team Name
PrepLens Labs

### Team Members
- Yuvaraj (Working independently as the sole developer and PM)

### Working Title
PrepLens: AI-Powered Assessment and Interview Question Generator

### Industry / Function
HR / Talent Acquisition / EdTech

### Target User
Hiring Managers, Technical Recruiters, and Educators who need to evaluate candidates or students.

### Current Workflow Problem
Drafting high-quality, relevant, and role-specific interview or assessment questions is highly manual, time-consuming, and inconsistent. Reviewers often reuse the same stale questions, leading to compromised assessment quality and a poor candidate experience. 

### Why This Matters
Poor or generic interview questions lead to bad hiring decisions, which are incredibly costly for businesses. Automating this workflow saves hours of highly-paid employee time per week while standardizing the quality of technical or behavioral assessments.

### Proposed AI Role
Generation and Copilot workflow. The AI generates contextual questions based on job descriptions, resumes, or specific topics, acting as a drafting assistant for the human reviewer.

### Why AI Instead of a Simpler Alternative
Rules-based systems or static question banks quickly become outdated and lack the flexibility to adapt to specific job requirements or individual candidate resumes. Generative AI is required to synthesize context (like a specific job description) into novel, tailored questions that can't be easily searched online by candidates.

### Human-in-the-Loop Design
The system operates as an assistant, not an autonomous agent. The human user must review the generated questions, edit them for nuance, approve the final list, and have the ability to regenerate options if the AI output misses the mark.

### Smallest Credible MVP
A web application where a user can input a specific topic or paste a short job description, and the system generates a structured list of 5 interview questions. The user can view a loading state, read the generated output, and click to approve or copy the questions. 

### Build vs Buy vs API View
**API View.** We will build a proprietary workflow and UI/UX layer but rely on existing foundation model APIs (e.g., Gemini or OpenAI) for the core text generation. Training a custom LLM from scratch for this MVP is economically unfeasible and unnecessary given the strong reasoning capabilities of off-the-shelf models.

### Main Risks
- **Hallucination/Irrelevance:** The AI generates questions that are technically inaccurate or unrelated to the prompt.
- **Bias:** The AI generates questions that favor certain demographics or backgrounds unintentionally.
- **Over-reliance:** The human reviewer blindly accepts low-quality questions without reviewing them, degrading the interview process.

### Success Evidence
- **Workflow Speed:** Reduction in time spent drafting a standard 5-question interview guide from 30+ minutes to under 5 minutes.
- **User Acceptance:** High rate of users copying or approving the generated questions without needing to completely rewrite them.

### Team Recommendation
**Proceed.** The workflow is real, the AI wedge is narrow and clearly defined, the MVP is actively in development, and the business value (time saved + better hiring signals) is significant.
