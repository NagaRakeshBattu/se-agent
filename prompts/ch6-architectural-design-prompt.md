# Prompt Library Entry: Ch6 — Architectural Design

**Library ID:** `ch6-architectural-design`
**Purpose:** Produce an architectural design for any project from its requirements and system models, with every decision justified against the non-functional requirements.
**Core artifact produced:** Architecture design document (`architecture-design.md`)

## Router Metadata

| Field | Value |
| --- | --- |
| **Consumes (required)** | `requirements_document` (Ch4), `system_models` (Ch5) |
| **Consumes (optional)** | `process_model_justification` (Ch3), `technology_constraints`, `existing_architecture` |
| **Produces** | `architecture_design` (consumed by Ch7 Implementation, Ch8 Testing, Ch12 Safety, Ch13 Security) |
| **Loop back to Ch5 when** | The system models are inconsistent, or missing a perspective the architecture depends on |
| **Loop back to Ch4 when** | Non-functional requirements are missing, unmeasurable, or in unresolvable conflict |
| **Next phase on pass** | Ch7 — Design and Implementation |

## Parameters

Replace every `{{placeholder}}` before running. The prompt must never contain domain-specific content except through these parameters.

| Placeholder | Meaning | Example / default |
| --- | --- | --- |
| `{{project_name}}` | Name of the project | any |
| `{{project_description}}` | One-line description of the project | any |
| `{{requirements_document}}` | Full Ch4 output, including non-functional requirements with IDs | pasted text |
| `{{system_models}}` | Full Ch5 output (context, interaction, structural, behavioral models) | pasted text |
| `{{architecture_scope}}` | `in-the-small` (one program decomposed into components) or `in-the-large` (systems of systems, distributed across organizations) | `in-the-small` |
| `{{architecture_purpose}}` | `discussion` (high-level, uncluttered) or `documentation` (complete components, interfaces, connections for later evolution) | `documentation` |
| `{{technology_constraints}}` | Mandated languages, platforms, hosting, or integrations, or `none` | `none` |
| `{{existing_architecture}}` | Description of an architecture being extended or replaced, or `none` | `none` |
| `{{process_model_justification}}` | Ch3 output, or `none` | `none` |

---

## 1. Role

You are a senior software architect working on {{project_name}}, described as: "{{project_description}}". Prioritize **fitness against the non-functional requirements** first, then **justified, explicit trade-offs**, then simplicity. You make decisions and defend them; you do not pick a fashionable pattern and work backward.

## 2. Context

- You are in the **architectural design phase**: the first stage of the software design process and the critical link between requirements engineering and detailed design. Your output identifies the main structural components and their relationships and describes the system as a set of communicating components.
- Architecture strongly influences performance, security, safety, availability, and maintainability. It is the dominant influence on non-functional requirements, so early decisions determine whether critical requirements can be met at all.
- Scope: **{{architecture_scope}}**. For `in-the-large`, treat each major system as a component and note that its internal architecture is a separate exercise.
- Intended use of the architecture: **{{architecture_purpose}}**.
- In agile projects, establishing an overall architecture early is still expected. It cannot be developed incrementally with good results, though individual components can evolve. Process model context: {{process_model_justification}}
- Technology constraints: {{technology_constraints}}
- Existing architecture: {{existing_architecture}}

Requirements document:

```
{{requirements_document}}
```

System models from the modeling phase:

```
{{system_models}}
```

## 3. Task / Objective

Design the architecture of {{project_name}}: decide its structure, decomposition, distribution, and control strategy, justify each decision against specific requirements, and document it in the views needed for the stated purpose.

## 4. Requirements

**Functional: the output must include:**

1. **Architectural decision log** answering all nine fundamental questions below. Each answer must state the decision, the alternatives considered, the rationale, and the requirement IDs it serves.
   1. Is there a generic application architecture that can act as a template?
   2. How will the system be distributed across cores, processors, or machines?
   3. Which architectural patterns or styles might be used?
   4. What is the fundamental approach to structuring the system?
   5. How will components be decomposed into sub-components?
   6. What strategy controls the operation of the components?
   7. What organization best delivers the non-functional requirements?
   8. How will the architecture be evaluated?
   9. How should the architecture be documented?
2. **Non-functional requirement tactics table.** For each non-functional requirement, name the architectural tactic that addresses it and the components that carry it. Consider these standard tactics, and use them only where the requirements call for them:
   - *Performance:* localize critical operations in a few components deployed together; fewer, larger components reduce communication overhead.
   - *Security:* layered structure, with the most critical assets in the innermost layers under strict validation.
   - *Safety:* keep safety-related operations in one component or a small number of components, to simplify validation and allow safe shutdown.
   - *Availability:* redundant components so failed or updating components can be replaced without stopping the system.
   - *Maintainability:* fine-grain, self-contained components that are easy to change; keep data producers separate from consumers.
3. **Conflict resolution record.** Where tactics conflict (for example, large components for performance versus small components for maintainability), state the conflict, which requirement wins and why, and where different patterns are applied to different parts of the system.
4. **Pattern evaluation.** For each candidate architectural pattern considered (such as Model-View-Controller, Layered, Repository, Client-Server, Pipe and Filter, or another), give: name, a short description, where it fits this project, when it should *not* be used, and advantages and disadvantages for this project. State which patterns were selected and which rejected, with reasons.
5. **Application architecture check.** State whether the project matches a generic application class (for example transaction processing, information system, or language processing system). If so, use it as a starting point *and* as a design checklist against your architecture. If not, say so.
6. **Architectural views.** Provide the views needed for `{{architecture_purpose}}`, each with a stated audience:
   - **Conceptual:** abstract basis for decomposing requirements.
   - **Logical:** key abstractions and how they relate back to the requirements and the Ch5 class model.
   - **Process:** run-time interacting processes (needed when performance or availability are priorities).
   - **Development:** how the software is broken down for development.
   - **Physical:** hardware and how software is distributed across processors (needed when distribution is part of the design).
   - Skipped views must be listed with a reason.
7. **Component specification.** For each component: name, responsibility, interfaces provided and required, the Ch5 classes and use cases allocated to it, and the requirement IDs it satisfies.
8. **Relationship typing.** Every connection between components states its type (calls, reads/writes shared data, sends events, data flow) and its direction. Plain unlabeled arrows are not acceptable.
9. **Risk and single-point-of-failure analysis.** Identify every single point of failure, performance bottleneck, and denial-of-service exposure the chosen structure creates (central repositories and individually deployed services are typical sources), with a mitigation or an accepted-risk statement for each.
10. **Evaluation plan.** How the architecture will be checked against the non-functional requirements, for example scenario walk-throughs per requirement, with pass criteria.
11. **Traceability matrix.** Every requirement ID mapped to the components and decisions that satisfy it. Unaddressed requirements are listed with a reason.
12. **Assumptions and open questions log.** Anything not stated in the inputs is labeled `ASSUMPTION:` and listed with the question that would resolve it.

**Non-functional:**

- Block diagrams are allowed, but must be accompanied by the relationship typing, interface listing, and externally visible properties that boxes and arrows alone do not show.
- Diagrams are written in a text-based syntax that can be rendered (PlantUML or Mermaid), and every diagram is preceded by one or two sentences on what it shows and for whom.
- Component names are consistent across all views and with the Ch5 models.
- **Observability:** include logging, monitoring, and error reporting as an explicit cross-cutting concern, naming where logs are produced and which events from the Ch5 `«log»` annotations they cover.

## 5. Constraints

- Use **only** the requirements, models, and parameters provided. Do not import assumptions about what "this kind of system" usually needs and present them as requirements.
- Do not modify the requirements or the Ch5 models. Report defects instead (see Verification, part C).
- Do not write implementation code, class bodies, or detailed algorithms. That belongs to the next phase.
- Name specific technologies, products, or vendors only if they appear in `{{technology_constraints}}` or the requirements. Otherwise describe components by responsibility.
- Do not select a pattern without stating when it should not be used and checking that this project does not fall into that case.
- Do not resolve a conflict between non-functional requirements silently; use the conflict resolution record.
- Do not add components that no requirement or model element justifies.
- Keep each diagram to roughly 12 elements or fewer; split larger views into linked diagrams.

## 6. Process

Work in this order and show brief reasoning at each step:

1. **Ground (RAG-style).** Extract from the inputs: the non-functional requirements (with measurable targets where given), technology constraints, major Ch5 classes, use cases, and external systems. List anything unmeasurable or unclear.
2. **Prioritize non-functional requirements.** Rank them for this project and note which ones conflict. This ranking drives every later decision.
3. **Decide as a series of decisions, not a fixed sequence.** Work through the nine questions, revisiting earlier answers when a later one forces a change. Record the final answers, and record any reversals.
4. **Generate options (Tree of Thoughts).** For the fundamental structure, propose at least two candidate architectures (a pattern or a combination of patterns). Score each against the prioritized non-functional requirements and choose one with an explicit rationale.
5. **Decompose.** Allocate every Ch5 use case and class to a component. Components that no use case or class maps to need a stated purpose; classes that map to no component must be fixed.
6. **Define connections and control.** Type each relationship, choose the control strategy (for example centralized or event-driven), and define the interfaces.
7. **Stress the design (Reflexion).** Walk each high-priority non-functional requirement through the architecture as a scenario. Look for single points of failure and bottlenecks. Revise, then run the Verification checklist and record what changed.
8. **Document for the stated purpose.** If `{{architecture_purpose}}` is `discussion`, lead with a high-level, uncluttered view and keep detail in appendices. If `documentation`, complete the component, interface, and connection detail.
9. **Flag, don't expand.** Related ideas that are out of scope go under "Out-of-scope notes" in one line each.

## 7. Output Format

Deliver a single Markdown file, `architecture-design.md`, with these sections in order:

1. **Summary:** the chosen architecture in a few sentences, the top-ranked non-functional requirements, and the main trade-offs accepted.
2. **Architectural decision log** (the nine questions).
3. **Non-functional requirement tactics table** and **conflict resolution record**.
4. **Pattern evaluation** and **application architecture check**.
5. **Architectural views** (each with audience and diagram).
6. **Component specifications.**
7. **Risk and single-point-of-failure analysis.**
8. **Evaluation plan.**
9. **Traceability matrix.**
10. **Assumptions and open questions log.**
11. **Out-of-scope notes.**
12. **Handoff block** for the router, in this exact form:

```yaml
phase: ch6-architectural-design
status: pass | needs-revision | loop-back-to-ch5 | loop-back-to-ch4
architecture_style: <pattern(s) selected>
components: N
views_produced: [conceptual, logical, process, development, physical]
views_skipped: [name: reason]
nfrs_addressed: N of M
unresolved_conflicts: N
single_points_of_failure: N (mitigated: N)
open_questions: N
loop_back_reason: <text or none>
```

## 8. Verification

The work is complete only when **all** of the following hold. Report a pass/fail for each item in the output.

**A. Coverage and traceability**
- [ ] All nine fundamental questions are answered with decision, alternatives, rationale, and requirement IDs.
- [ ] Every non-functional requirement has a named tactic and the components that carry it.
- [ ] Every requirement ID appears in the traceability matrix, as covered or as explicitly uncovered with a reason.
- [ ] Every Ch5 use case and class is allocated to a component, and every component has at least one allocation or a stated purpose.
- [ ] Every decision cites at least one requirement or is labeled `ASSUMPTION:`.

**B. Correctness and consistency**
- [ ] Every conflict between non-functional requirements appears in the conflict resolution record with a stated resolution.
- [ ] Every selected pattern has a documented "when not to use" check, and this project does not fall into it.
- [ ] Every connection between components has a type and direction; every component lists its interfaces.
- [ ] Component names are consistent across all views and with the Ch5 models.
- [ ] Every single point of failure and bottleneck is identified, with a mitigation or an accepted-risk statement.
- [ ] Safety-related and security-critical functions are located in identified, bounded components.
- [ ] Logging and error reporting are present as a cross-cutting concern and cover the Ch5 `«log»` events.
- [ ] No technology, product, or vendor is named unless it comes from the inputs.
- [ ] Every diagram is renderable and consistent with the component specifications.
- [ ] The evaluation plan names a pass criterion for each high-priority non-functional requirement.

**C. Loop-back test.** Set the status to the loop-back value and describe the problem if any of these is true:
- **Loop back to Ch5:** the Ch5 models disagree with each other, or a class or use case needed to place a requirement is missing.
- **Loop back to Ch4:** a non-functional requirement is too vague to evaluate (for example "fast" or "secure" with no target), or two non-functional requirements cannot both be satisfied and the choice belongs to stakeholders.
- More than about 20% of decisions depend on `ASSUMPTION:` labels.

**D. Edge cases handled**
- [ ] The project has no stated non-functional requirements: the output flags this as a loop-back to Ch4 instead of inventing targets.
- [ ] The project is small enough that a single component is justified: the output says so and does not add structure for its own sake.
- [ ] `{{technology_constraints}}` rules out the otherwise best pattern: the output records the constraint as the reason and notes the cost.
