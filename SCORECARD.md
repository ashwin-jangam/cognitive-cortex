# Agent Scorecards

These are the scores for every agent against [`RUBRIC.md`](RUBRIC.md). They are **design-review** scores: what each `agent.md` is designed to do. Behavioural test results (rubric §5) will be added once the test suites exist.

> **Review status:** the author assessed these scores on branch `agent-fixes`. Rubric §1 requires a second reviewer before they count as final. Until behavioural pass rates are recorded, every band is **provisional**.

## Summary

| Agent | Deterministic | Verification | Schema | Reusability | Context | Total | Band | Before fixes |
|---|---|---|---|---|---|---|---|---|
| [Assessment Copilot](agents/assessment-copilot/agent.md) | 19 | 19 | 19 | 19 | 19 | **95** | Exemplary | 59 (failed G1; raw 65) |
| [Grading Assistant](agents/grading-assistant/agent.md) | 19 | 20 | 20 | 19 | 18 | **96** | Exemplary | 93 |
| [Self-Study Tutor](agents/self-study-tutor/agent.md) | 19 | 19 | 20 | 19 | 19 | **96** | Exemplary | 81 |
| [Assignment Advisor](agents/assignment-advisor/agent.md) | 20 | 20 | 19 | 18 | 20 | **97** | Exemplary | 59 (failed G4; raw 87) |

---

### Agent Rubric: assessment-copilot

**Hard gates:** G1 ✅ (the folder was renamed to match) · G2 ✅ (Hard Rule 2) · G3 ✅ (§7) · G4 ✅ (Hard Rule 6, `status`) · G5 ✅ (Hard Rule 5)

| Principle | C1 | C2 | C3 | C4 | Total | Evidence |
|---|---|---|---|---|---|---|
| Deterministic | D1 5 | D2 5 | D3 4 | D4 5 | 19 | §3, §4, §5 |
| Verification | V1 5 | V2 4 | V3 5 | V4 5 | 19 | §2 source priority, Hard Rule 1, §6.2–6.3 |
| Output schema | O1 4 | O2 5 | O3 5 | O4 5 | 19 | §7.1–7.2 |
| Reusability | R1 5 | R2 4 | R3 5 | R4 5 | 19 | §2, §8, §7.2 → Grading Assistant |
| Context | Z1 5 | Z2 5 | Z3 4 | Z4 5 | 19 | 216 lines, §9 |

**Deductions:** none. (The example's total was corrected from 14 to 12 marks, and an unverifiable textbook quote was replaced with a placeholder.)
**Total:** 95 · **Band:** Exemplary (provisional) · **Behavioural pass rate:** not yet measured
**Remaining gaps:**
1. D3: no rule for how the objective round-robin interacts with the difficulty allocation.
2. V2: when no source material is given, a question cites only an objective, not an excerpt.
3. O1 / Z3: no cap on SAQ model-answer length, and the full student paper is always output for sets of 20 questions or fewer.
4. R2: the command-word ladder assumes English.

### Agent Rubric: grading-assistant

**Hard gates:** G1 ✅ · G2 ✅ (Hard Rule 7) · G3 ✅ (§8) · G4 ✅ (`status`, §9 "Approve") · G5 ✅ (Hard Rule 5)

| Principle | C1 | C2 | C3 | C4 | Total | Evidence |
|---|---|---|---|---|---|---|
| Deterministic | D1 5 | D2 4 | D3 5 | D4 5 | 19 | §3, §4, §6 Tally, §7 tie-break |
| Verification | V1 5 | V2 5 | V3 5 | V4 5 | 20 | §2 source priority, Hard Rule 2, Gates 1–2 |
| Output schema | O1 5 | O2 5 | O3 5 | O4 5 | 20 | §8.1–8.3, CSV |
| Reusability | R1 5 | R2 4 | R3 5 | R4 5 | 19 | §2, §9, takes Copilot JSON as input |
| Context | Z1 4 | Z2 4 | Z3 5 | Z4 5 | 18 | 244 lines, §10 batching |

**Deductions:** none.
**Total:** 96 · **Band:** Exemplary (provisional) · **Behavioural pass rate:** not yet measured
**Remaining gaps:**
1. D2: no rule for an answer written under the wrong part (e.g., the answer to (c) placed in (b)).
2. R2: no guidance for answers written in a language other than the assessment's.
3. Z1 / Z2: the "evidence or zero" rule is restated in three places, and there's no rule for what to discard between batches.

### Agent Rubric: self-study-tutor

**Hard gates:** G1 ✅ · G2 ✅ (§6 "Never invent") · G3 ✅ (§5, §8) · G4 ✅ (Hard Rules 1 and 5, §9, Hard Rule 7) · G5 ✅

| Principle | C1 | C2 | C3 | C4 | Total | Evidence |
|---|---|---|---|---|---|---|
| Deterministic | D1 5 | D2 5 | D3 5 | D4 4 | 19 | §3 session and turn, §4, precedence order |
| Verification | V1 5 | V2 4 | V3 5 | V4 5 | 19 | §2 sources, §6 tables, turn gate, §8 gate |
| Output schema | O1 5 | O2 5 | O3 5 | O4 5 | 20 | §5 template, §8 recap and JSON |
| Reusability | R1 5 | R2 4 | R3 5 | R4 5 | 19 | §2, §10, `practice_bank`, shared tags |
| Context | Z1 5 | Z2 4 | Z3 5 | Z4 5 | 19 | 222 lines, §11 |

**Deductions:** none. (The graded-work rule is now stated once and referenced elsewhere.)
**Total:** 96 · **Band:** Exemplary (provisional) · **Behavioural pass rate:** not yet measured
**Remaining gaps:**
1. D4: reply word counts are self-counted, with no second check.
2. V2 / R2: verification for humanities subjects relies on the student supplying the supporting text.
3. Z2: the running log has no size cap for very long sessions.

### Agent Rubric: assignment-advisor

**Hard gates:** G1 ✅ · G2 ✅ (Hard Rule 4) · G3 ✅ (§8) · G4 ✅ (Hard Rule 1, §10 safeguarding *(added)*, Hard Rule 7) · G5 ✅

| Principle | C1 | C2 | C3 | C4 | Total | Evidence |
|---|---|---|---|---|---|---|
| Deterministic | D1 5 | D2 5 | D3 5 | D4 5 | 20 | §5, §3 (including the no-limit row), tie-breaks, §7 |
| Verification | V1 5 | V2 5 | V3 5 | V4 5 | 20 | §2 sources, Hard Rules 2 and 4, Gates 1–2, `confidence` |
| Output schema | O1 4 | O2 5 | O3 5 | O4 5 | 19 | §8.1–8.3 |
| Reusability | R1 5 | R2 4 | R3 4 | R4 5 | 18 | §2, §9, Copilot rubrics, Tutor handoff |
| Context | Z1 5 | Z2 5 | Z3 5 | Z4 5 | 20 | 225 lines, §11 |

**Deductions:** none.
**Total:** 97 · **Band:** Exemplary (provisional) · **Behavioural pass rate:** not yet measured
**Remaining gaps:**
1. O1: the student template shows only the `draft` stage, with no layouts for `brainstorm` or `outline`.
2. R2: the default criteria assume an argument, which fits `creative_writing` poorly.
3. R3: no row for a teacher-provided exemplar, or for drafts in another language.

---

## Next steps

1. **Independent review** of all four scorecards (rubric §1, step 4).
2. **Behavioural suites:** 10–15 test cases per agent (rubric §5). Report each pass rate next to its design score.
3. Close the remaining gaps listed above. Each one is worth 1 point.
