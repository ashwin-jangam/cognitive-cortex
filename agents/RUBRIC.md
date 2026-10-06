# Agent Design Rubric

This rubric is for scoring every `agents/<name>/agent.md` in this repo before it is merged, and again whenever it changes significantly. It measures how well an agent is **designed** against our five principles:

| # | Principle | Points |
|---|---|---|
| 1 | Deterministic Execution | 20 |
| 2 | Verification & Anti-Hallucination | 20 |
| 3 | Output Schema Quality | 20 |
| 4 | Reusability & Real Utility | 20 |
| 5 | Zero Context Bloat | 20 |
| | **Total** | **100** |

Each principle has **4 criteria**, and each criterion is scored **0–5**. Scores of 0, 3 and 5 have descriptions below. Use 1, 2 or 4 when the agent falls between two descriptions.

> **What this rubric measures:** what the agent is designed to do. It doesn't measure how the agent actually behaves when run. A high score here is a prerequisite for testing, not a replacement for it (see §5).

---

## 1. How to Score

1. **Check the hard gates first** (§2). If any gate fails, the agent is capped at **59 (Not ready)**, whatever its criterion scores.
2. **Score each criterion 0–5** against the descriptions in §3. Every score needs **evidence**: a section number or line range in the `agent.md`. A score without a cited location doesn't count.
3. **Add up the scores.** Each principle's total is the sum of its four criteria (out of 20), and the overall total is the sum of the five principles (out of 100).
4. **Use two reviewers** for new agents. If they differ by 2 or more points on any criterion, they discuss it and agree a score. Otherwise, use the lower of the two scores.
5. **Record the result** using the scorecard template (§6) in the PR description.

### Score bands

| Total | Band | Merge decision |
|---|---|---|
| 90–100 | **Exemplary** | Merge |
| 80–89 | **Strong** | Merge; record the minor fixes as follow-ups |
| 70–79 | **Adequate** | Fix the lowest-scoring principle before merging |
| 60–69 | **Weak** | Rework needed |
| < 60 | **Not ready** | Don't merge |

**Floor rule:** if any one principle scores below 10 out of 20, the agent can't be rated higher than **Adequate**, whatever its total.

---

## 2. Hard Gates (all must pass)

| Gate | Requirement |
|---|---|
| **G1 Identity** | The file begins with a header block (frontmatter) containing a `name` that matches the folder name, and a `description` saying *what the agent does* and *when to use it*. |
| **G2 No licence to fabricate** | Nothing in the file allows or encourages inventing facts, sources, citations, curriculum codes, data or quotes. |
| **G3 Defined output** | At least one output format is specified, as a template or a schema. |
| **G4 Human control** | Teacher-facing agents keep the teacher's approval on anything with consequences (marks, reports, communication with parents). Student-facing agents include rules for academic integrity, privacy and safeguarding. |
| **G5 Privacy** | The agent never asks for, keeps, or repeats a student's personal data beyond what the task strictly needs. |

---

## 3. Criteria and Score Descriptions

### Principle 1: Deterministic Execution (20)

*The same input should produce the same kind of decision and output, every time.*

| Criterion | 0 | 3 | 5 |
|---|---|---|---|
| **D1 Ordered procedure** | No procedure, or only loose prose | Numbered steps, but the order is ambiguous or some steps are optional without saying when | Numbered steps marked "in order, no skipping". Every branch point says which step comes next |
| **D2 Decision tables** | Recurring judgments are left to "use your judgment" | Some judgments are encoded as rules; the edge cases aren't covered | Every recurring judgment is a condition → action table, including edge cases (blank, missing, out-of-scope or conflicting input) |
| **D3 Defaults & tie-breaks** | Optional inputs have no defaults; ordering, rounding and ties are unspecified | Defaults are given for most inputs; some ordering or rounding rules are missing | Every optional input has a default. Rounding, ranking keys, tie-breaks and caps are stated explicitly |
| **D4 Computation offloaded** | Counting, arithmetic or dates are estimated | The agent is told to "calculate carefully" | Counting, arithmetic and dates use a code tool when one is available, and a stated double-check method (e.g., adding in both directions) when it isn't. "Estimate" or "about" is banned for measured values |

### Principle 2: Verification & Anti-Hallucination (20)

*Every claim the agent makes can be traced back to a source and checked.*

| Criterion | 0 | 3 | 5 |
|---|---|---|---|
| **V1 Declared grounding** | No statement of where content may come from | The source is named (e.g., "curriculum"), with no fallback | The permitted sources are listed in priority order, with a defined behaviour for when a source is missing or doesn't cover the request |
| **V2 Checkable evidence** | Conclusions have no supporting evidence | Evidence is encouraged but optional | Every judgment or claim needs a **verbatim** quote or citation (with length limits) that a reviewer can check. No evidence means no credit or no claim |
| **V3 Pre-output gates** | No self-check | A general "review your work" instruction | Named checklists (gates) run before output, each item is pass/fail, and failures are fixed or escalated to the user |
| **V4 Uncertainty & abstention** | The agent is never told it may be unsure | It's told to "flag uncertainty" | Confidence levels or flags with set meanings, scripted fallback wording, a list of things it must never invent (sources, codes, numbers, quotes), and a route for sending unresolved cases to a human |

### Principle 3: Output Schema Quality (20)

*Outputs are predictable, easy to parse, and fit the people and systems that use them.*

| Criterion | 0 | 3 | 5 |
|---|---|---|---|
| **O1 Human format** | Free-form output | A suggested structure | An exact template with fixed sections, section order and length limits (adjusted by grade or audience where relevant) |
| **O2 Machine schema** | None | JSON shown as an example only | A JSON schema with required fields, types, lists of allowed values, and a `schema_version` |
| **O3 Schema rules** | None | Some field notes | Explicit rules covering empty values versus `null`, how derived fields are calculated, when a field is left out, and word limits for each field |
| **O4 Consumer fit** | One output for everyone | Teacher and student outputs are separated | Outputs are separated by audience, there are exports for the systems that consume them (CSV, LMS), and status or approval fields track the workflow |

### Principle 4: Reusability & Real Utility (20)

*The agent works across classrooms and saves real time in real workflows.*

| Criterion | 0 | 3 | 5 |
|---|---|---|---|
| **R1 Input contract** | Inputs are implied | Inputs are listed | A table marks each input as required or optional, with defaults, and an "ask once, in one message, only for what's missing" rule |
| **R2 Domain-agnostic** | Hardcoded to one subject, grade or board | Adapts with some effort | Parameterised by subject, grade, curriculum and language. Examples are clearly marked as illustrations |
| **R3 Workflow coverage** | Only the single happy path | A few follow-up requests are handled | A request → action table covers common follow-ups, revisions, edge cases and handoffs to other agents |
| **R4 Interoperability** | Standalone | Mentions related agents | Consumes or produces formats that other agents in this repo use (e.g., Assessment Copilot rubrics feeding into Grading Assistant) |

### Principle 5: Zero Context Bloat (20)

*Every token earns its place, in the prompt and in the outputs.*

| Criterion | 0 | 3 | 5 |
|---|---|---|---|
| **Z1 Lean prompt** | More than 400 lines, or heavy repetition or filler | 250–400 lines, with some duplication | 250 lines or fewer. Every section can be acted on, and nothing is said twice (a rule stated once is referenced afterwards, not restated) |
| **Z2 Minimal state** | Silent on what to remember | Partly specified | Lists exactly what to keep between turns or items, and what to discard |
| **Z3 Terse outputs** | Restates the inputs and echoes whole documents back | Some limits | Doesn't restate inputs. Quotes have length limits. Large inputs get a summary first, with detail on request |
| **Z4 Efficient interaction** | Asks one question at a time, or asks about things that have defaults | Mostly batched | One batched clarification message at most. No questions about things with defaults. No meta-commentary unless asked |

---

## 4. Common Deductions

Apply these after scoring each criterion. A criterion can't go below 0.

| Finding | Deduction |
|---|---|
| Weak words in a rule ("try to", "where possible", "generally") with no condition attached | −1 on the criterion the rule belongs to |
| An example that contradicts the agent's own rules (e.g., a sample over its own word limit, or a wrong total) | −2 on O1 or O2 |
| A rule that conflicts with another rule, with no stated precedence | −2 on D1 |
| Instructions that depend on tools not every host has, with no fallback | −1 on D4 or R2 |
| References to files or paths that don't exist | −1 on R4 |

---

## 5. Measuring Actual Behaviour (strongly recommended)

A design score is what the agent is meant to do. Behavioural tests show what it actually does. For each agent, keep **10–15 test cases** (an input plus pass/fail checks) that cover:

- the normal case, one or more edge cases (blank, missing or out-of-scope input), and one or more adversarial cases (e.g., "just give me the answer", "write it for me")
- one or more checks for every hard rule in the agent, for example: "no mark of 1 or more without a quote", "no final answer while at hint level L2", "no named source that wasn't provided"

Report the result as `pass rate %` next to the design score. Treat an agent with a design score of 80 or more but a pass rate under 85% as **Adequate**, until the failing rules are fixed.

---

## 6. Scorecard Template

Copy this into the PR description:

```markdown
### Agent Rubric: <agent-name> (agents/<name>/agent.md @ <commit>)

**Hard gates:** G1 ☐ G2 ☐ G3 ☐ G4 ☐ G5 ☐

| Principle | C1 | C2 | C3 | C4 | Total /20 | Evidence (§ or lines) |
|---|---|---|---|---|---|---|
| Deterministic Execution | D1 _ | D2 _ | D3 _ | D4 _ | _ | |
| Verification & Anti-Hallucination | V1 _ | V2 _ | V3 _ | V4 _ | _ | |
| Output Schema Quality | O1 _ | O2 _ | O3 _ | O4 _ | _ | |
| Reusability & Real Utility | R1 _ | R2 _ | R3 _ | R4 _ | _ | |
| Zero Context Bloat | Z1 _ | Z2 _ | Z3 _ | Z4 _ | _ | |

**Deductions:** <finding → criterion → −n>
**Total:** __ / 100 · **Band:** ____ · **Behavioural pass rate:** __% (n cases)
**Top 3 fixes:** 1. … 2. … 3. …
**Reviewers:** @… @…
```

---

## 7. Author Checklist (before requesting a review)

- [ ] The header block has a `name` matching the folder and a `description` with a "Use when…" trigger
- [ ] Hard rules come first, are numbered, and have no exceptions
- [ ] An input contract table includes defaults
- [ ] There's a numbered procedure, and decision tables for recurring judgments
- [ ] Counting and arithmetic go to a code tool or a double-check method
- [ ] Claims require verbatim evidence, and gates run before output
- [ ] There's a human template and a JSON schema with allowed values and rules for empty fields
- [ ] There's a request → action table for common follow-ups
- [ ] The file is 250 lines or fewer, with no repeated rules, and says what state to keep
- [ ] The examples follow the agent's own rules (check the numbers, limits and dates)
