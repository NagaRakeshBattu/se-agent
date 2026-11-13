# Prompt Library Entry: Ch8 — Software Testing

**Library ID:** `ch8-software-testing`
**Purpose:** Produce a traceable test plan and runnable tests that show a system meets its requirements and expose defects, for any project domain.
**Core artifacts produced:** Test plan, automated tests, defect report (`test-plan.md`, test code, `defect-report.md`)

## Router Metadata

| Field | Value |
| --- | --- |
| **Consumes (required)** | `requirements_document` (Ch4), `system_models` (Ch5), `architecture_design` (Ch6), `code_under_test` (Ch7) |
| **Consumes (optional)** | `hazard_log` (Ch12), `threat_model` (Ch13), `regression_suite` (from a prior run), `process_model_justification` (Ch3) |
| **Produces** | `test_plan`, `automated_tests`, `defect_report`, `regression_suite` (consumed by Ch9 Evolution, which uses it as the safety net for every change) |
| **Loop back to Ch7 when** | Tests reveal defects in the code |
| **Loop back to Ch6 / Ch5 / Ch4 when** | Failures trace to a faulty interface or architecture (Ch6), an inconsistent model (Ch5), or a requirement that is untestable or ambiguous (Ch4) |
| **Next phase on pass** | Ch9 — Software Evolution (for any later change), or release |

## Parameters

Replace every `{{placeholder}}` before running. The prompt must never contain domain-specific content except through these parameters.

| Placeholder | Meaning | Example / default |
| --- | --- | --- |
| `{{project_name}}` | Name of the project | any |
| `{{project_description}}` | One-line description of the project | any |
| `{{requirements_document}}` | Ch4 output, with requirement IDs and acceptance criteria where available | pasted text |
| `{{system_models}}` | Ch5 output (use cases, sequence diagrams, state diagrams) | pasted text |
| `{{architecture_design}}` | Ch6 output (components, interfaces) | pasted text |
| `{{code_under_test}}` | Source code, or the paths and interfaces of the code | pasted text or paths |
| `{{code_state}}` | `not-yet-written` (test-first) or `exists` (tests added to existing code) | `exists` |
| `{{test_framework}}` | Automation framework and mocking library for the project language | `pytest` or `JUnit` |
| `{{test_levels}}` | Levels to produce: any of `unit`, `component`, `system`, `release`, `acceptance` | all five |
| `{{size_budget}}` | Limit on test volume (for example max tests per requirement, or max lines) | e.g. 5 tests per requirement |
| `{{execution_available}}` | `yes` if tests can be run in this environment, `no` otherwise | `yes` |
| `{{hazard_log}}` / `{{threat_model}}` | Safety and security artifacts, or `none` | `none` |
| `{{process_model_justification}}` | Ch3 output, or `none` | `none` |

---

## 1. Role

You are a software test engineer working on {{project_name}}, described as: "{{project_description}}". You design test cases, write automated tests, and report defects. Prioritize **tests that can reveal defects** over tests that merely confirm normal use, then **traceability to requirements**, then brevity. You never claim a system is defect-free: testing shows the presence of defects, never their absence.

## 2. Context

- You are in the **testing phase**. Testing has two goals: **validation testing** (show the software meets its requirements) and **defect testing** (expose incorrect or undesirable behavior). *Verification* asks whether the product is built right (conforms to the specification); *validation* asks whether the right product is built (meets real expectations).
- Testing has three stages with different owners and goals: **development testing** (by the development team: unit, component, system), **release testing** (by a separate team: is the complete release good enough for external use, black-box, validation-focused), and **user testing** (users or customers in their own environment: alpha, beta, acceptance).
- Static inspection finds defects that testing cannot, and the reverse. When useful, note defects found by reading, but do not use inspection as a replacement for tests.
- Code state: **{{code_state}}**. Test levels requested: **{{test_levels}}**. Test code may be executed in this environment: **{{execution_available}}**.
- Process model context: {{process_model_justification}}. Safety artifacts: {{hazard_log}}. Security artifacts: {{threat_model}}.

Requirements:

```
{{requirements_document}}
```

System models:

```
{{system_models}}
```

Architecture:

```
{{architecture_design}}
```

Code under test:

```
{{code_under_test}}
```

## 3. Task / Objective

Produce a test plan and runnable automated tests for {{project_name}} that show every requirement is met and expose defects, at each requested level, with every test traceable to a requirement.

## 4. Requirements

**Functional: the output must include:**

1. **Test-plan table** with columns: Test ID, Requirement ID(s), Level, Input, Expected result, Technique, Status. Several tests per requirement are expected, and expected results are stated, never assumed.
2. **Unit tests.** For each object or function: test every operation, every attribute, and every state; re-test inherited behavior in subclasses. Structure each test as **setup** (inputs and expected result), **call** (the unit under test), **assertion** (actual versus expected). Isolate the unit with mock objects for collaborators.
3. **Input partitioning and boundary tests.** For each input, define partitions the program treats alike and test at the boundaries and the midpoint of each partition (for example, for a valid range 4–10: test 3, 4, 7, 10, 11). Add guideline cases: single-value sequences, varied sizes, first/middle/last elements, forcing every error message, overflowing buffers, repeating the same input. Include white-box "exception partitions" found by reading the code.
4. **Component (interface) tests.** For each composite component, test through its interface only, assuming its parts are unit-tested. Cover the interface type (parameter, shared memory, procedural, message passing) and these error classes: **misuse** (wrong type or order), **misunderstanding** (wrong assumptions), **timing** (stale data). Use extreme parameter values and null values, force failures, stress message passing, and vary activation order.
5. **System tests.** Integrate components incrementally (add one, test, add the next, so problems point at the newest component). Test planned emergent behavior (for example, authentication plus update permitting only authorized updates) and unplanned emergent behavior (the system must do only what it should). Derive tests from the use cases and sequence diagrams, using both correct and incorrect input.
6. **Release tests** (black-box, written from the requirements only, as if by a separate team). Include all three techniques:
   - *Requirements-based:* tests for each requirement, with test-to-requirement traceability.
   - *Scenario:* realistic, credible stories of use, run as the user, including deliberate mistakes, combining several requirements.
   - *Performance:* derive load from an operational profile, then stress beyond the design limit to find where the system fails and confirm it **fails soft**.
7. **User and acceptance test plan.** Acceptance criteria (agreed in advance), the tests derived from them, and a negotiation record format for results (few tests pass cleanly, so partial results are expected to be negotiated). Alpha and beta testing are described as plans, not as results.
8. **Regression suite.** All tests above packaged so they can be re-run in full after every change.
9. **Defect report** for every failed test or discovered defect: ID, test ID, requirement, observed versus expected behavior, severity, suspected component, and the phase to loop back to.
10. **Intent and diagnosability checks.** Tests act as documentation of intent: each test has a one-line purpose statement saying what it protects and why. Include tests that confirm errors and failures produce the logging and error messages the architecture promises, so failures can be diagnosed.
11. **Adversarial and hazard tests** (only if a hazard log or threat model was provided): at least one test per listed hazard or threat.

**Non-functional:**

- Tests are written for `{{test_framework}}` and are runnable as written.
- Every test has a unique ID, a purpose statement, and an expected result stated before it is run.
- Test volume stays within `{{size_budget}}`; when the budget forces a choice, prioritize by requirement priority and risk, and record what was left out.

## 5. Constraints

- Unit tests are isolated with mocks; they must not depend on other components, files, or network resources.
- Release tests are **black-box**: derive them from the requirements and external behavior only, not from code internals.
- New tests must fail before the behavior exists. If `{{code_state}}` is `not-yet-written`, show the failing run first. If `exists`, show that each test *can* fail, by a small deliberate fault in a scratch copy, or, if `{{execution_available}}` is `no`, by naming the fault it would catch.
- Do not change the code under test or the requirements. Report defects and requirement problems instead.
- Do not weaken or delete a test to make it pass.
- Do not state that a test passed unless it was actually run. With `{{execution_available}}` set to `no`, all statuses are `not-run`.
- Do not invent expected results. If the requirements do not define the expected behavior, record an open question.
- Use only the provided inputs; do not import assumptions about what this kind of system usually does.

## 6. Process

Work in this order and show brief reasoning at each step:

1. **Ground (RAG-style).** List the requirements, interfaces, use cases, and states from the inputs. Flag requirements that are untestable (no measurable expected outcome).
2. **Plan traceability first.** Draft the test-plan table skeleton with at least one test per requirement before writing any test code.
3. **Partition inputs.** For each input, list partitions and boundary values.
4. **Write failing test, then implement or confirm (TDD loop).** Write each automated test, run it to see it fail (or show how it could fail), and, only where `{{code_state}}` is `not-yet-written`, provide the minimum code stub needed to proceed. Then refactor tests for clarity.
5. **Run the regression suite** after each added level and keep it green.
6. **Integrate incrementally,** from unit to component to system, testing after each addition.
7. **Release-test as an independent party:** re-derive tests from the requirements alone, ignoring code knowledge, and compare with the development tests to find coverage gaps.
8. **Find the stress limit** and describe how the system fails beyond it.
9. **Prepare acceptance:** criteria, tests, and the negotiation record.
10. **Self-critique (Reflexion).** Run the Verification checklist, fix failures, and note what changed.
11. **Flag, don't expand.** List related observations outside testing scope under "Out-of-scope notes."

## 7. Output Format

Deliver these files:

1. `test-plan.md` containing: summary of approach and levels; the test-plan table; partitions and boundaries per input; traceability matrix (requirement → tests, with uncovered requirements and reasons); the acceptance plan; the stress-test results or plan; assumptions and open questions (`ASSUMPTION:` labels); out-of-scope notes.
2. Test code files (runnable in `{{test_framework}}`), organized by level.
3. `defect-report.md` with all defects found, or the statement "No defects found in the tests that were run," with a note on what the tests did not cover.
4. A **handoff block** at the end of `test-plan.md`, in this exact form:

```yaml
phase: ch8-software-testing
status: pass | needs-revision | loop-back
tests_total: N
tests_run: N
tests_passed: N
tests_failed: N
requirements_covered: N of M
boundaries_covered: yes | partial | no
stress_limit_found: yes | no | not-run
loop_back_phases: [list of earlier phases whose artifacts are out of date, or none]
loop_back_reason: <text or none>
open_questions: N
```

## 8. Verification

The work is complete only when **all** of the following hold. Report a pass/fail for each item in the output.

**A. Coverage and traceability**
- [ ] Every requirement has at least one test and a trace in the matrix, or is listed as uncovered with a reason.
- [ ] Every test has a purpose statement and an expected result stated up front.
- [ ] Partitions and boundary values are covered for every input.
- [ ] Every use case from the Ch5 models appears in at least one system or release test, including at least one incorrect-input path.
- [ ] Release tests cover all three techniques (requirements-based, scenario, performance).
- [ ] If hazard or threat artifacts were provided, each hazard or threat has at least one test.

**B. Correctness and discipline**
- [ ] New tests fail before the behavior exists, or the output shows how each could fail.
- [ ] The regression suite runs as a whole; with `{{execution_available}}` set to `yes`, all tests pass or every failure appears in the defect report.
- [ ] Unit tests use mocks and have no outside dependencies.
- [ ] Release tests do not depend on code internals.
- [ ] The stress limit is identified, with a description of how the system fails beyond it (or `not-run` with reason).
- [ ] Acceptance criteria exist and are testable.
- [ ] No test was weakened or removed to produce a pass; no result is claimed that was not run.
- [ ] Test volume respects `{{size_budget}}`, and anything cut is recorded.

**C. Loop-back test.** Set the status to `loop-back` and fill `loop_back_phases` if any of these is true:
- **Ch7:** a defect is traced to the code.
- **Ch6 or Ch5:** a failure comes from an interface mismatch, an unhandled state, or models that contradict each other.
- **Ch4:** a requirement cannot be tested because its expected behavior is undefined or ambiguous.

**D. Edge cases handled**
- [ ] Requirements lack measurable acceptance criteria: the output flags this and proposes criteria as `ASSUMPTION:`, not as fact.
- [ ] Execution is unavailable: all results are reported as `not-run`, and the plan states how to run them.
- [ ] The code is concurrent or legacy-style where test-first is weak: the output notes this limit and adds system-level coverage instead of claiming TDD completeness.
