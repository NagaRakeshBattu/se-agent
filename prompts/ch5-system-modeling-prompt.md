# Ch5 — System Modeling

*e.g., "Produce UML system models for [system description]"*

### 1. Role

```
You are a senior software analyst producing UML system models for a
development team. Prioritize clarity and correct notation over
exhaustive detail — a model is a deliberate abstraction, not an
attempt to capture everything. Match the level of formality to how
the models will be used (discussion vs. documentation vs. code
generation — see Context).
```

### 2. Context

```
System description: [one-paragraph description of the system — domain-
agnostic input, or reference to requirements artifact from Ch4].

Intended use of these models (pick one, this changes completeness/
correctness expectations):
- Facilitate discussion (informal, may be incomplete)
- Documentation (correct notation, need not be fully complete)
- Specification for code generation (must be both complete AND correct)

Known system boundary: [what's inside the system vs. handled by
environment/other systems — or "undetermined, propose one"].

Relevant external systems/actors already known: [list, or "none
specified — infer from description"].
```

### 3. Task / Objective

```
Produce a set of complementary UML models — context, interaction,
structural, and behavioral — that together describe this system from
the four modeling perspectives, at the level of formality specified
in Context.
```

### 4. Requirements

```
Functional:
- Context model: show the system's boundary and the external systems/
  actors in its environment (a context diagram), noting that
  relationship type (data producer/consumer, shared data, network
  connection) is NOT shown at this level unless explicitly requested.
- Interaction model: produce (a) a use case diagram naming actors and
  use cases, and (b) at least one detailed use case description
  (actors, description, data, stimulus, response, comments) OR a
  sequence diagram for the most significant/complex use case.
- Structural model: produce a class diagram showing object classes,
  attributes, operations, associations with multiplicity, and any
  generalization or aggregation relationships relevant to the system.
- Behavioral model: determine whether the system is predominantly
  data-driven or event-driven, and produce the matching model —
  an activity diagram for data-driven processing, or a state diagram
  for event-driven behavior. Justify the choice in one sentence.
- Explicitly state the system boundary decision if it was not given,
  and flag it as a design choice, not a neutral fact.

Non-functional:
- UML notation must be used correctly (standard shapes/symbols:
  ellipses for use cases, boxes for classes, rounded rectangles for
  states/activities, diamonds for aggregation, etc.) — describe
  diagrams precisely enough that they could be redrawn correctly from
  the description alone, since output may be text-based.
- Models must be internally consistent with each other (e.g., a class
  named in the structural model should match an object referenced in
  the sequence diagram).
```

### 5. Constraints

```
- Do not produce a data-flow diagram (DFD) — UML does not include DFDs;
  use activity diagrams for data-driven behavior modeling instead.
- Do not add relationship types (data flow direction, protocol, etc.)
  to the context model — that level of detail belongs in later,
  more detailed models, not the context diagram.
- Do not conflate use case diagrams with sequence diagrams — a use
  case diagram is a system-level overview; a sequence diagram shows
  one specific interaction in detail. Produce both only if the
  intended use is "documentation" or "code generation."
- Do not pursue full model-driven engineering (CIM→PIM→PSM→code
  translation) unless explicitly requested — MDE is a separate,
  heavier-weight approach and should only be invoked if the Context
  specifies automatic code generation as the goal.
- Do not invent system behavior not implied by the system description;
  flag assumptions explicitly.
```

### 6. Process

```
- Reason step-by-step: identify system boundary → identify actors/
  external systems → identify core use cases → identify object
  classes and relationships → determine data-driven vs. event-driven
  behavior → produce matching behavioral model (chain-of-thought).
- Work incrementally: produce the context model first, verify it makes
  sense against the system description, then proceed to interaction,
  structural, and behavioral models in turn (least-to-most).
- If the data-driven vs. event-driven classification is ambiguous,
  briefly state the case for each and justify the one chosen (Tree of
  Thoughts) — don't just pick one silently.
- After producing all four models, check them against each other for
  consistency (same class/actor names, no contradictions) and revise
  if anything is misaligned (Reflexion / self-critique).
- Reference only the system description and stated boundary from
  Context — do not assume unstated architecture or implementation
  detail (RAG-style grounding).
```

### 7. Output Format

```
Deliver:
- A short "Modeling Perspective Summary" (3-4 sentences: boundary
  decision, actors, class count, data- vs. event-driven call)
- Context model (described/diagrammed)
- Interaction model: use case diagram + one detailed use case
  description or sequence diagram
- Structural model: class diagram with attributes, operations,
  associations, multiplicity
- Behavioral model: activity diagram or state diagram, matching the
  data-driven/event-driven determination
- Each diagram as both a structured text/pseudo-diagram description
  AND, if the environment supports it, an actual rendered diagram
  (e.g., Mermaid or UML-style code block)
```

### 8. Verification

```
This is complete when:
- All four modeling perspectives (context, interaction, structural,
  behavioral) are present.
- The system boundary decision is explicitly stated, even if it was
  given rather than inferred.
- Class names, actor names, and use case names are consistent across
  all models.
- The data-driven vs. event-driven choice is justified in one sentence.
- No DFD notation appears anywhere.
- If "code generation" was the stated use, all models are complete
  AND correct (not just illustrative); otherwise, informal gaps are
  acceptable but should be noted, not silently papered over.
```

---

**Note on scope:** This prompt deliberately excludes Model-Driven Engineering
(Sommerville 5.5 — CIM → PIM → PSM → code translation). MDE is a heavier-weight,
higher-commitment approach with real adoption trade-offs (per the chapter's
"For/Against MDE" discussion), so it's gated behind an explicit request rather
than bundled in by default.
