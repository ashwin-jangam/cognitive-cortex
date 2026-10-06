# Cognitive Cortex

**Building AI-Ready → AI-Enabled → AI-Native Schools.**

Cognitive Cortex is an AI transformation partner for schools. This repo contains our public site (`index.html`) and the **AI agents** we build for teachers and students. Each agent is a single, self-contained `agent.md` file, and each one is scored against a published 100-point design rubric before it ships.

---

## The Agents

| Agent | For | What it does | Design score |
|---|---|---|---|
| [**Assessment Copilot**](agents/assessment-copilot/agent.md) | Teachers | Drafts curriculum-grounded MCQ, short-answer and logical-reasoning questions, with a verified answer key and rubric | **95** |
| [**Grading Assistant**](agents/grading-assistant/agent.md) | Teachers | Suggests rubric marks, each backed by a quote from the student's answer, plus feedback and a class misconception report. The teacher approves every mark | **96** |
| [**Self-Study Tutor**](agents/self-study-tutor/agent.md) | Students | A Socratic tutor that guides students with escalating hints, without giving away answers, and writes a session summary for the teacher | **96** |
| [**Assignment Advisor**](agents/assignment-advisor/agent.md) | Students | Coaches essays and projects from first idea to final check, against the teacher's rubric. It never ghostwrites | **97** |

Scores are from a design review (see [`SCORECARD.md`](SCORECARD.md)). They are provisional until an independent review and behavioural testing are complete.

### How they work together

```
Assessment Copilot ──rubric + answer key (JSON)──▶ Grading Assistant ──misconception tags──▶ teacher
        │                                                                 ▲
        └──practice_bank──▶ Self-Study Tutor ──session summary────────────┤
                                   ▲                                      │
           concept gaps ───────────┘   Assignment Advisor ──teacher_log──┘
```

Every agent uses the same conventions: kebab-case misconception tags, a `schema_version` on every JSON output, and a `status` field that keeps the teacher in control.

---

## Our Design Standard

Generic AI tools are unpredictable, invent facts, return free-form text and pad their answers. Classrooms need the opposite. Every agent in this repo is built and scored on **five design principles**, worth 20 points each:

| # | Principle | What it means | What it looks like in our agents |
|---|---|---|---|
| 1 | **Deterministic Execution** | The same input produces the same kind of decision, every time | Numbered procedures, decision tables, explicit tie-breaks, and computation done by code or a double-check method |
| 2 | **Verification & Anti-Hallucination** | Every claim can be traced back to a source and checked | "Evidence or zero" marking, verbatim quotes, blind solving of the answer key, pre-output gates, and lists of things the agent must never invent |
| 3 | **Output Schema Quality** | Outputs are predictable, easy to parse, and fit their audience | Fixed templates, versioned JSON schemas with fixed allowed values, separate student and teacher views, and LMS/CSV exports |
| 4 | **Reusability & Real Utility** | It works across classrooms and saves real time | Input contracts with defaults, curriculum-agnostic parameters, follow-up request tables, and agents that feed each other |
| 5 | **Zero Context Bloat** | Every token earns its place | Prompts of 250 lines or fewer, explicit state to keep and discard, capped quotes, and one batched question at most |

### How an agent is scored

1. **Hard gates (all must pass).** The header matches the folder, nothing permits fabrication, an output format is defined, humans stay in control, and student privacy is protected. **A failed gate caps the agent at 59.**
2. **20 criteria scored 0–5** (4 per principle), each with fixed descriptions of what a 0, 3 and 5 look like. **Every score must cite where in the `agent.md` the evidence is.**
3. **Deductions** for vague wording ("try to"), examples that break the agent's own rules, conflicting rules, and stale references.
4. **Bands:** 90–100 Exemplary · 80–89 Strong · 70–79 Adequate · 60–69 Weak · below 60 Not ready. An agent can't rate above Adequate if any single principle scores below 10.
5. **Behavioural tests:** 10–15 test cases per agent. If a design score of 80 or more comes with a pass rate below 85%, the agent is treated as Adequate.

The full rubric, scorecard template and author checklist are in [`RUBRIC.md`](RUBRIC.md).

---

## Using an Agent

Each `agent.md` starts with a `name` and `description` header, so it works in most agent hosts:

- **Claude Code:** copy the file to `.claude/agents/<name>.md` in your project (or `~/.claude/agents/`), and it becomes available as a subagent.
- **Other hosts / API:** use the file's body as the system prompt. Hosts with a code-execution tool get the strongest determinism (counting, totals, dates).

All four agents work out of the box with sensible defaults. Provide your curriculum material for the best grounding.

---

## Contributing a New Agent

1. Create `agents/<name>/agent.md`. The header's `name` must match the folder name.
2. Work through the **author checklist** in [`RUBRIC.md` §7](RUBRIC.md#7-author-checklist-before-requesting-a-review).
3. Score the agent using the **scorecard template** ([`RUBRIC.md` §6](RUBRIC.md#6-scorecard-template)), and paste it into your PR description.
4. Two reviewers score it independently. A new agent needs **80 or more** (Strong) to merge, and must pass every hard gate.

---

## Repo Structure

```
.
├── index.html          # Public site (hash-routed single page)
├── assets/             # Logo and icons
├── agents/
│   ├── assessment-copilot/agent.md
│   ├── grading-assistant/agent.md
│   ├── self-study-tutor/agent.md
│   └── assignment-advisor/agent.md
├── RUBRIC.md           # 100-point agent design rubric
├── SCORECARD.md        # Current scores for every agent
└── LICENSE
```

## License

See [LICENSE](LICENSE).
