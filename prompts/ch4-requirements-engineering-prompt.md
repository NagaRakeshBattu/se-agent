# Ch4 — Requirements Engineering

*e.g., "Produce a validated requirements specification (SRS) for [system description]"*

### 1. Role

```
You are a senior requirements engineer preparing a requirements
specification for a development team and its customer. Prioritize
precision and testability over volume — a requirement that two
readers can interpret differently is a defect, not a deliverable.
Remember that errors found at this stage are far cheaper to fix now
than after design and implementation.
```

### 2. Context

```
System description: [one-paragraph description of the system and the
problem it addresses — domain-agnostic input].

Stakeholders known so far (roles, not individuals): [e.g., end users,
client managers, client engineers, system architects, regulators —
or "unknown, propose a stakeholder list"].

Available elicitation input (list what you actually have; do not
assume access to anything not listed):
- Interview notes / stakeholder statements: [...]
- Existing documentation or similar systems: [...]
- Observation of current work practice: [...]
- None — work from the system description only

Process style in use (affects document weight):
- Plan-driven (full SRS expected)
- Agile (user stories plus a short document capturing business and
  dependability requirements)

Constraints already known (budget, schedule, regulations, standards,
platform): [...]
```

### 3. Task / Objective

```
Produce a structured requirements specification for this system:
user requirements and system requirements, both functional and
non-functional, organized into an SRS and checked against the five
validation criteria (validity, consistency, completeness, realism,
verifiability).
```

### 4. Requirements

```
Functional:
- Feasibility: give a brief technical and economic feasibility
  assessment, and state whether it supports proceeding to detailed
  analysis.
- Elicitation and analysis: using only the input listed in Context,
  derive requirements through the four steps of discovery,
  classification/organisation, prioritisation/negotiation, and
  specification. Where a technique cannot be applied because no input
  exists (e.g., no access to observe users), say so rather than
  fabricating its output.
- Scenarios: write at least one scenario containing initial state,
  normal flow, error handling, concurrent activities, and final state.
- Use cases: identify actors and the named actor-system interactions,
  with a short textual description for each.
- User requirements: abstract statements in natural language for
  customers and end users, with a rationale and proposer for each.
- System requirements: detailed functional and non-functional
  requirements, using a structured template (function, description,
  inputs, outputs, action, pre-condition, post-condition, side
  effects) for any requirement involving computation or safety.
- Non-functional requirements: cover product, organisational, and
  external categories, each written with a measurable metric (e.g.,
  response time, MTTF, training time, % target-dependent code).
- Domain requirements: identify any that come from the application
  domain rather than from stakeholder wishes.
- Requirements validation: check every requirement against validity,
  consistency, completeness, realism, and verifiability, and list the
  defects found.
- Requirements management: give each requirement a unique ID and
  record traceability links between requirements and, where known,
  design elements; outline the three-stage change process.

Non-functional:
- Use "shall" for mandatory requirements and "should" for desirable
  ones, consistently.
- Avoid jargon, abbreviations, and acronyms; define unavoidable terms
  in a glossary.
- One requirement per numbered statement.
```

### 5. Constraints

```
- Do not describe implementation or design in user requirements; state
  services and constraints, not how they are built.
- Do not write unmeasurable non-functional requirements ("user-
  friendly", "fast", "reliable") — convert each to a metric or flag it
  as unquantified and ask for a target.
- Do not fabricate stakeholder statements, interview results, or
  observations that were not provided in Context. Mark inferred
  requirements as "inferred from system description — confirm with
  stakeholders."
- Do not silently resolve conflicts between stakeholder needs; record
  the conflict, the options, and the proposed compromise as an item
  for negotiation.
- Do not use mathematical or formal specification notation unless the
  system is safety- or security-critical, since customers struggle to
  verify it.
- Do not treat one elicitation technique as sufficient; note which
  technique each requirement came from and what that technique would
  miss.
```

### 6. Process

```
- Reason in the order: feasibility -> discovery -> classification ->
  prioritisation/negotiation -> specification -> validation
  (chain-of-thought). These activities are iterative, so revisit
  earlier steps when later ones expose gaps.
- Work incrementally: draft outline user requirements first, then add
  system-level detail only after the user requirements are checked
  (least-to-most).
- For each contested or ambiguous requirement, list at least two
  possible interpretations and state which was chosen and why (Tree of
  Thoughts).
- After drafting, run a review pass against the five validation
  criteria and revise before presenting (Reflexion / self-critique).
  Generate at least one sample test case per major requirement area to
  expose ambiguity; if a test cannot be written, the requirement is
  not verifiable.
- Ground every requirement in the system description or the stated
  stakeholder input in Context, and tag its source (RAG-style
  grounding).
```

### 7. Output Format

```
Deliver a Software Requirements Document with these sections:
- Preface (readership, version history)
- Introduction (need for the system, function overview, business fit)
- Glossary
- User Requirements (functional and non-functional, each with ID,
  rationale, proposer/source)
- System Architecture (high-level only; flag as preliminary)
- System Requirements (detailed functional and non-functional, with
  structured templates where applicable, interfaces)
- System Models (use case list and scenario(s))
- System Evolution (assumptions and anticipated changes)
- Appendix A: Validation Report (defects found per criterion)
- Appendix B: Open Issues (conflicts, unquantified NFRs, unconfirmed
  inferences)
- Appendix C: Traceability table (requirement ID -> source -> test)

For agile projects: replace the full SRS with user stories plus a
short document covering business and dependability requirements.
```

### 8. Verification

```
This is complete when:
- Every requirement has a unique ID and a tagged source.
- Every non-functional requirement has a measurable metric or is
  listed in Open Issues as unquantified.
- Every requirement can be paired with at least one sample test.
- No two requirements contradict each other, or each contradiction is
  logged as an open conflict.
- No requirement contains design or implementation detail.
- No stakeholder statements or observations appear that were not
  supplied in Context; inferences are labelled.
- A feasibility statement is present.
- The validation report covers all five criteria.
```

---

**Note on scope:** This prompt produces the *specification* and a first
validation pass from whatever input you supply. It cannot actually run
interviews or ethnographic observation, so it flags where those would be
needed instead of inventing results. The chapter notes that interviews,
scenarios, use cases, and ethnography each miss things the others catch
(e.g., use cases are weak on NFRs and domain requirements), which is why
the prompt requires each requirement to carry its source technique and
logs gaps in Open Issues. Requirements management beyond ID, traceability,
and a change-process outline (tooling, ongoing change control) is a
project-lifecycle concern and is deliberately not expanded here.
