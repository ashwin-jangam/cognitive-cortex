# Grading Assistant Behavior Tests

Run each input in a fresh session. A case passes only when every listed check passes.

## 1. Evidence-backed marking

**Input:** Provide a rubric with a 1-mark criterion, the answer "Metal conducts heat, so the steel handle gets hotter," and ask for marking.

**Pass checks:**
- Awards the mark only with an exact verbatim evidence span and a criterion-linked justification.
- Does not add criteria or deduct for spelling, grammar or presentation unless the rubric requires it.
- Teacher-facing output is a suggestion pending approval.

## 2. Blank and unidentified responses

**Input:** Provide valid questions and rubric, two student responses (one blank), and names instead of student IDs.

**Pass checks:**
- Assigns IDs in input order and never repeats supplied names.
- Treats the blank as zero, flags `blank_response`, and never awards marks without evidence.
- Computes student and class totals from the actual marks.

## 3. Missing rubric

**Input:** Provide questions and student answers but no rubric or answer key; ask to grade immediately.

**Pass checks:**
- Stops grading and asks for the missing rubric/key in one message.
- May offer to draft a rubric, but does not grade until the teacher approves it.

## 4. Partial credit and class consistency

**Input:** Provide a multi-mark criterion with partial credit enabled and several responses, then ask to re-mark one response as "too harsh."

**Pass checks:**
- Applies the criterion in the same way across all students and uses only allowed 0.5 increments for partial marks.
- Re-marks that criterion for every student, reports changes and recalculates affected totals and class summaries.
- Double-checks totals and percentages.

## 5. Valid answer missing from rubric

**Input:** Provide a response that appears valid but is not covered by the rubric or model answer; ask to approve all marks.

**Pass checks:**
- Does not use subject knowledge to award marks; sets the criterion to zero pending review and flags `outside_rubric_valid_answer` and `needs_teacher_review`.
- Marks affected totals and class statistics provisional, with `needs_review: true`.
- Does not approve or export final results until the teacher resolves the item.

## Design score record

| Principle | Criterion scores | Total /20 | Evidence |
|---|---|---:|---|
| Deterministic Execution | D1 5 · D2 5 · D3 4 · D4 5 | 19 | §3; §4; §§2, 4, 6; §6 Tally |
| Verification & Anti-Hallucination | V1 5 · V2 5 · V3 5 · V4 5 | 20 | §2; Hard Rule 2 and §4; Gates 1–2; Hard Rule 7 and §4 |
| Output Schema Quality | O1 5 · O2 5 · O3 5 · O4 5 | 20 | §8.1; §8.2; §8.2 schema rules; §§5, 8–9 |
| Reusability & Real Utility | R1 5 · R2 5 · R3 5 · R4 5 | 20 | §2; §2; §§4, 9; overview and §8.2 |
| Zero Context Bloat | Z1 4 · Z2 5 · Z3 5 · Z4 5 | 19 | 249-line prompt; §10; §§5, 8, 10; §10 |

**Total:** 98/100 · **Hard gates:** pass · **Behavior cases:** 5 defined; not run.
