# Agent Design Rubric

Use this rubric to score each `sample agents/<name>/agent.md` before release and whenever it changes significantly. It measures how well an agent is **designed** against our five principles:

| # | Principle | Points |
|---|---|---|
| 1 | Deterministic Execution | 20 |
| 2 | Verification & Anti-Hallucination | 20 |
| 3 | Output Schema Quality | 20 |
| 4 | Reusability & Real Utility | 20 |
| 5 | Zero Context Bloat | 20 |
| | **Total** | **100** |

Each principle has **4 criteria**, and each criterion is scored **0–5**. Scores of 0, 3 and 5 have descriptions below.

---

## 1. How to Score

1. **Check the hard gates first** (§2). If any gate fails, the agent is capped at **59 (Not ready)**, whatever its criterion scores.
2. **Score each criterion 0–5** against the descriptions in §3. Every score needs **evidence**: a section number or line range in the `agent.md`. A score without a cited location doesn't count.
3. **Add up the scores.** Each principle's total is the sum of its four criteria (out of 20), and the overall total is the sum of the five principles (out of 100).
4. **Record the result** using the scorecard template (§6) alongside the agent change.

### Score bands

| Total | Band | Next step |
|---|---|---|
| 90–100 | **Exemplary** | Ready to release |
| 80–89 | **Strong** | Ready to release; note minor follow-ups |
| 70–79 | **Adequate** | Fix the lowest-scoring principle before release |
| 60–69 | **Weak** | Rework, then score again |
| < 60 | **Not ready** | Do not release |

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

*Same input, consistent decisions and output.*

| Criterion | 0 | 3 | 5 |
|---|---|---|---|
| **D1 Ordered procedure** | No procedure | Numbered steps, but order or optional steps are unclear | Steps must be followed in order; each branch specifies what comes next |
| **D2 Decision tables** | Recurring decisions rely on judgment | Some decisions have rules; edge cases are missing | Condition → action rules cover recurring decisions and blank, missing, out-of-scope or conflicting inputs |
| **D3 Defaults & tie-breaks** | Defaults and decision rules are unspecified | Most defaults are set; some ordering, rounding or tie rules are missing | Defaults, rounding, ranking, tie-breaks and caps are explicit |
| **D4 Computation offloaded** | Counts, arithmetic or dates are estimated | Told to calculate carefully | Use code when available; otherwise specify a double-check method. Never estimate measured values |

### Principle 2: Verification & Anti-Hallucination (20)

*Claims are grounded in sources and can be checked.*

| Criterion | 0 | 3 | 5 |
|---|---|---|---|
| **V1 Declared grounding** | Sources are unspecified | Sources are named, but no fallback is defined | Permitted sources are prioritized; missing or insufficient sources have a defined fallback |
| **V2 Checkable evidence** | Claims lack evidence | Evidence is optional | Each claim or judgment requires checkable, length-limited verbatim evidence or citation; without evidence, abstain or award no credit |
| **V3 Pre-output gates** | No self-check | General review instruction | Named pass/fail checks run before output; failures are fixed or escalated |
| **V4 Uncertainty & abstention** | No allowance for uncertainty | Told to flag uncertainty | Define confidence flags, fallback wording, forbidden inventions (e.g., sources, codes, numbers, quotes) and human escalation |

### Principle 3: Output Schema Quality (20)

*Outputs are consistent, parseable and suited to their users and systems.*

| Criterion | 0 | 3 | 5 |
|---|---|---|---|
| **O1 Human format** | Free-form | Suggested structure | Exact sections, order and length limits, adapted to audience or grade as needed |
| **O2 Machine schema** | No schema | JSON example only | Versioned JSON schema defines required fields, types and allowed values |
| **O3 Schema rules** | No field rules | Some field notes | Define empty vs. `null`, derived fields, omission conditions and field length limits |
| **O4 Consumer fit** | Same output for everyone | Teacher and student outputs differ | Audience-specific outputs, needed exports (e.g., CSV/LMS) and workflow status or approval fields |

### Principle 4: Reusability & Real Utility (20)

*Works across classrooms and supports useful workflows.*

| Criterion | 0 | 3 | 5 |
|---|---|---|---|
| **R1 Input contract** | Inputs are implied | Inputs are listed | Mark inputs required or optional, set defaults, and ask once only for missing information |
| **R2 Domain-agnostic** | Fixed to one subject, grade or board | Some adaptation is possible | Parameters cover subject, grade, curriculum and language; examples are labeled illustrative |
| **R3 Workflow coverage** | Happy path only | Some follow-ups are handled | Request → action rules cover follow-ups, revisions, edge cases and handoffs |
| **R4 Interoperability** | Standalone | Related agents are mentioned | Uses or produces formats shared with repo agents (e.g., rubrics passed to Grading Assistant) |

### Principle 5: Zero Context Bloat (20)

*Keep prompts and outputs concise without losing useful detail.*

| Criterion | 0 | 3 | 5 |
|---|---|---|---|
| **Z1 Lean prompt** | Over 400 lines, or heavy repetition/filler | 250–400 lines with some duplication | At most 250 actionable lines; state each rule once and refer back to it |
| **Z2 Minimal state** | Memory needs are unspecified | Partly specified | State exactly what to keep between turns or items and what to discard |
| **Z3 Terse outputs** | Repeats inputs or whole documents | Some output limits | Don't repeat inputs; cap quotes; summarize large inputs and provide detail on request |
| **Z4 Efficient interaction** | Repeated or one-at-a-time clarification questions, including about defaults | Mostly batched, with avoidable follow-ups | At most one batched clarification; don't ask about defaults or add unrequested meta-commentary. Socratic teaching questions are not clarifications |

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

A design score is what the agent is meant to do. Behavioural tests show what it actually does. For each agent, keep **5 test cases** (an input plus pass/fail checks) that cover:

- the normal case, one or more edge cases (blank, missing or out-of-scope input), and one or more adversarial cases (e.g., "just give me the answer", "write it for me")
- one or more checks for every hard rule in the agent, for example: "no mark of 1 or more without a quote", "no final answer while at hint level L2", "no named source that wasn't provided"

Report the result as `pass rate %` next to the design score. Treat an agent with a design score of 80 or more but a pass rate under 85% as **Adequate**, until the failing rules are fixed.

---

## 6. Scorecard Template

Use this template to record your review with the agent change:

```markdown
### Agent Rubric: <agent-name> (sample agents/<name>/agent.md @ <commit>)

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
```
