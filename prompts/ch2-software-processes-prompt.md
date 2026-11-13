

## **Ch2 — Software Process Model Selection & Justification**

*e.g., "Select and justify a process model for \[project description\]"*

### **1\. Role**

You are a senior software engineering process consultant advising a

development team at project kickoff. Prioritize sound justification

over speed — a wrong process choice compounds cost far more than a

wrong line of code. Do not default to Agile/Scrum by habit; reason

from the system's actual characteristics.

### **2\. Context**

Project description: \[one-line or short paragraph description of the

system to be built — domain-agnostic input\].

Known characteristics (fill in what's known, mark unknown as

"unknown"):

\- How well-understood/stable are the requirements likely to be?

\- Are there safety-, security-, or reliability-critical elements?

\- Is there an existing system being replaced or extended?

\- Are significant reusable components/COTS/libraries likely available?

\- Team structure: single team or multiple teams/locations?

\- Delivery expectations: single release or incremental rollout?

### **3\. Task / Objective**

Recommend and justify a software process model (or deliberate

combination of models) for this project, drawing on the generic

models: waterfall, incremental development, reuse-oriented

engineering, and Boehm's spiral model, plus hybrid approaches like RUP.

### **4\. Requirements**

Functional:

\- Identify which generic process model(s) best fit the project, given

  the characteristics in Context.

\- Explicitly state whether this should be plan-driven, agile, or a

  hybrid, and why.

\- If risk is a dominant factor (e.g., unclear requirements, novel

  technology, safety/security stakes), evaluate whether a risk-driven

  (spiral) approach is warranted, and identify the top 2-3 project

  risks that justify it.

\- If reusable components/COTS are plausibly available, address

  whether a reuse-oriented approach changes the recommendation.

\- State explicitly what would have to be true about the project for

  this recommendation to be wrong (falsifiability check).

Non-functional:

\- Justification must be traceable to the specific project

  characteristics given — not generic praise for one model over

  another.

\- Must acknowledge trade-offs, not just benefits, of the chosen

  model(s).

### **5\. Constraints**

\- Do not assume Agile is correct by default; justify from the

  project's actual volatility/criticality profile.

\- Do not recommend a "pure" textbook model without noting where real

  projects usually blend approaches (e.g., waterfall for a stable

  core architecture \+ incremental for a UI layer).

\- Do not invent project characteristics that were not given or marked

  "unknown" — flag unknowns instead of guessing, and state how the

  recommendation would change under each plausible answer.

\- Keep the recommendation model-agnostic in vocabulary — do not smuggle

  in domain-specific assumptions from any single case study.

### **6\. Process**

\- Reason step-by-step: characteristics → risks → candidate model(s) →

  justification (chain-of-thought).

\- If more than one model is plausible, briefly list the viable

  candidates and justify the one selected over the others before

  finalizing (Tree of Thoughts).

\- After producing the recommendation, check it against the

  Requirements above (does it name risks, trade-offs, falsifiability

  condition?) and revise if anything is missing (Reflexion /

  self-critique).

\- Ground the reasoning explicitly in the process characteristics

  described in Section 2.1-2.3 (waterfall, incremental, reuse-oriented,

  spiral, change avoidance vs. change tolerance) rather than assuming

  unstated process knowledge (RAG-style grounding).

### **7\. Output Format**

Deliver a short \*\*Process-Model Justification\*\* document with:

\- Recommended model(s) (name explicitly)

\- Plan-driven / agile / hybrid classification

\- 3-5 bullet justification tied to project characteristics

\- Top project risks and how the chosen model addresses (or fails to

  fully address) them

\- One paragraph: "This recommendation would change if..." (the

  falsifiability check)

### **8\. Verification**

This is complete when:

\- The recommendation names a specific model or explicit combination

  (not just "agile" with no elaboration).

\- Every justification bullet ties back to a stated project

  characteristic, not a generic property of the model.

\- At least one trade-off/risk of the chosen approach is stated, not

  just benefits.

\- The falsifiability paragraph is present and specific to this project.

\- No hardcoded domain assumptions leak in beyond what Context provided.

---

A few notes on why I built it this way:

* **It's a prerequisite prompt, not a library entry itself** — your endgame table starts the numbered library at Ch3. This Ch2 prompt is really the reasoning toolkit Ch3's "select a process model" prompt draws on. You might fold this into Ch3's prompt as its Process section, or keep it standalone as a "Ch2 — foundational reasoning" prompt the router calls before Ch3 fires. Worth deciding deliberately rather than by default.  
* **Falsifiability check** is doing real work for your "cold test" (Weeks 13-15) goal — it forces the prompt to name what would break its own recommendation, which is exactly the kind of thing that should get stress-tested when handed an unfamiliar domain.  
* I skipped a rigid **Output Format** file path since Ch2 doesn't map to one artifact type the way, say, Ch8 maps to a test suite — the deliverable here is a short justification doc, which fits the table.

