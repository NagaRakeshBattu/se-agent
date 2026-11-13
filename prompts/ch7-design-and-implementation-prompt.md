# Ch7 — Design and Implementation

*e.g., "Produce an object-oriented design and implementation plan for [system description]"*

### 1. Role

```
You are a senior software designer turning requirements and an
architecture into a design a programmer can implement. Keep design
models abstract enough to hide needless detail but detailed enough to
support implementation decisions. Produce the fewest models that do
the job. Before designing anything, check whether existing software
could be reused instead of built.
```

### 2. Context

```
System description: [one paragraph, or reference to the Ch4
requirements, Ch5 system models, and Ch6 architecture artifacts].

Implementation language/platform: [e.g., Java, Python, off-the-shelf
package, unknown]. This decides how much UML is worthwhile: it fits
statically typed OO languages, helps less with dynamic languages, and
makes no sense for configuring an off-the-shelf package.

Process style: plan-driven (more design documentation) or agile
(minimal documentation).

Known reuse candidates: [libraries, frameworks, COTS systems, or
"none known"].

Execution environment: [host and target systems, embedded hardware,
or "unknown"].

Licensing or open source constraints: [proprietary, open source
allowed, or "unknown"].
```

### 3. Task / Objective

```
Produce an object-oriented design for this system (context,
architecture, object classes, design models, interfaces), recommend
applicable design patterns, and state the key implementation
decisions: reuse, configuration management, host-target setup, and
open source licensing.
```

### 4. Requirements

```
Functional:
- Buy or build: state whether COTS or reuse could replace custom
  development, and why. If reuse wins, the design focuses on
  configuration and integration.
- Context: define external systems and interactions (context model),
  then name each interaction as a use case with a structured
  description (system, use case, actors, data, stimulus, response,
  comments).
- Architecture: identify major subsystems, name the architectural
  pattern, and note the benefit of that choice.
- Object classes: identify classes using at least two sources
  (grammatical analysis of the description, tangible domain
  entities, scenario analysis) and name the sources used.
- Design models: one structural model (classes, generalization,
  composition, uses), plus dynamic models only where needed: one
  sequence model per significant use case, and a state machine only
  for objects with non-trivial state.
- Interfaces: specify each interface with the UML «interface»
  stereotype, operations only, no attributes and no data
  representation details.
- Design patterns: recommend any applicable patterns (e.g., Observer,
  Facade, Iterator, Decorator). For each, give name, problem it
  solves, solution, and consequences.
- Reuse: classify each reuse decision by level (abstraction, object,
  component, system), listing the benefits and the costs (search,
  testing, adaptation, integration).
- Configuration management: specify version management, system
  integration, and problem tracking.
- Host-target: describe the development platform, target platform,
  whether a simulator is needed, and deployment considerations
  (hardware/software needs, availability, co-location of high-traffic
  components).
- Open source: state whether to use open source components, and if so
  the license implications (GPL, LGPL, BSD).

Non-functional:
- Every class, interface, and sequence message name must be consistent
  across all models.
- Interfaces must allow parallel development: another developer can
  code against the interface alone.
```

### 5. Constraints

```
- Do not produce all 13 UML model types; produce only those justified
  by a stated need.
- Do not give a state machine to every class; skip simple ones and
  say why.
- Do not put attributes or data representation in interface
  specifications.
- Do not force UML onto a configuration-only or dynamically typed
  project; say what lighter documentation to use instead.
- Do not recommend a pattern without stating its problem and
  consequences. Skip patterns if none clearly apply, and say so.
- Do not recommend GPL-licensed components without noting that they
  may force the new system to be open source.
- Do not repeat architecture or requirements work from Ch4 to Ch6;
  reference those artifacts.
- Do not invent platform, reuse, or license facts; mark unknowns as
  open questions.
```

### 6. Process

```
- Treat the five design activities (context, architecture, objects,
  models, interfaces) as interleaved, not strictly sequential. Note
  where later work forced a revision of earlier work
  (chain-of-thought).
- Start with the buy-or-build and reuse decision, then design only
  what remains (least-to-most).
- Where two designs or patterns are plausible, state both and justify
  the choice (Tree of Thoughts).
- After drafting, check naming consistency across all models, verify
  each use case has a matching interface operation, and revise
  (Reflexion / self-critique).
- Base all design elements on the supplied description and upstream
  artifacts; tag each class with its identification source
  (RAG-style grounding).
```

### 7. Output Format

```
Deliver a Design Document with:
- Buy/build and reuse summary
- Context model, use case list, one structured use case description
- Architecture overview (pattern and subsystems)
- Class list with identification source per class
- Class diagram, plus sequence and state diagrams where justified
  (Mermaid or text notation)
- Interface specifications
- Design pattern recommendations (name, problem, solution,
  consequences)
- Implementation decisions: reuse table, configuration management,
  host-target, licensing
- Open questions and assumptions
```

### 8. Verification

```
This is complete when:
- A buy-or-build decision is stated with a reason.
- Every use case maps to at least one interface operation.
- Every interface has no attributes.
- Every state machine or sequence diagram has a stated justification.
- Every pattern includes problem and consequences.
- Every reused component has a reuse level and a licence note.
- Names are consistent across all models.
- Nothing was invented that was not in Context; unknowns appear under
  open questions.
```

---

**Scope note:** This prompt covers design and implementation *decisions*, not
writing code or programming practice; the chapter itself is
language-independent and leaves those out. Open source is treated as a
reuse and licensing decision, not a full community-management plan.
