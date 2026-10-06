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

The full rubric, scorecard template and author checklist are in [RUBRIC.md](RUBRIC.md).

---

## Agent Scores

| Agent | Design score |
|---|---:|
| [Assessment Copilot](agents/assessment-copilot/agent.md) | 95/100 |
| [Grading Assistant](agents/grading-assistant/agent.md) | 96/100 |
| [Self-Study Tutor](agents/self-study-tutor/agent.md) | 96/100 |
| [Assignment Advisor](agents/assignment-advisor/agent.md) | 97/100 |

---

## Using an Agent

Choose an agent from the table above and use its `agent.md` file as the instructions for your AI assistant. Include relevant curriculum material in the conversation or the platform's knowledge/files area for the best grounding.

- **Gemini:** Create a Gem, then paste the contents of the chosen `agent.md` into its instructions. Add curriculum materials as knowledge files or provide them in the chat.
- **Claude:** Create a Project and paste the contents of the chosen `agent.md` into the project instructions. Add curriculum materials to the project or provide them in the chat. In Claude Code, copy the file to `.claude/agents/<name>.md` in your project (or `~/.claude/agents/`) to use it as a subagent.
- **OpenAI:** Create a custom GPT and paste the contents of the chosen `agent.md` into its Instructions. Add curriculum materials as Knowledge files or provide them in the chat.
- **Other hosts / API:** use the file's body as the system prompt. Hosts with a code-execution tool get the strongest determinism for counting, totals, and dates.

Availability and exact setup steps may vary by plan and platform. All four agents work out of the box with sensible defaults.
