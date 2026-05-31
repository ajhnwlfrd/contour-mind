# MVP Router Spec

## How to use this file

This file documents the router's thought process and operational behaviour.

**Full skill setup** — use the SKILL.md files in `claude/.claude/skills/` or `codex/.agents/skills/`. The skills give you slash commands and a clean invocation flow.

**Standalone spec-building** — if you just want spec-building behaviour in any project without the full skill setup, copy this file directly:
- Claude: save as `CLAUDE.md` in your project root.
- Codex: save as `AGENTS.md` in your project root.

The agent will follow the router logic, readiness checks, and closure gate defined below.

## Purpose
This skill helps software developers break down messy problems into clear, actionable specs by applying mental models invisibly during a discovery conversation.

The output of the skill is a structured markdown spec and a ready-to-use implementation prompt, with reduced cognitive load as the primary design goal.

## System role
This skill belongs to the **thinking layer** in a three-layer architecture: thinking layer for discovery and spec generation, orchestration layer for keeping the spec as a living document, and implementation layer where a coding agent executes scoped tasks.

The orchestration layer follows an OODA-style loop so the system can monitor output, detect drift, and self-heal while implementation is underway.

## Router design
The skill uses one adaptive router rather than multiple separate routers.

That router should classify the current problem mode, choose the next best mental model, ask targeted discovery questions, and reroute as new information appears.

## Level 1 groups
The router is organized into three top-level groups.

| Group | Tools |
|---|---|
| Core decomposition | Abstraction laddering, issue trees, first principles, inversion, second-order thinking  |
| Diagnostic extensions | Iceberg, Ishikawa, ladder of inference, concept map  |
| Execution choice extensions | Decision matrix, prioritization tools such as Eisenhower Matrix or Impact-Effort Matrix  |

## MVP scope
The MVP is **Level 2**, not just the Level 1 grouping.

Level 2 defines the operational behavior for each tool: trigger signals, primary question pattern, follow-up questions, exit condition, and likely next route.

Each turn should normally use one primary tool, with at most one handoff to a second tool if the first tool reveals a new need.

## Level 2 routing table

### Core decomposition

| Tool | Trigger signals | Primary question pattern | Exit condition | Next likely route |
|---|---|---|---|---|
| Abstraction laddering | Problem is vague, framed as a solution, or mixes goals and tasks  | "What are you really trying to achieve?" then "Why does that matter?" or "How would that work in practice?"  | Clear problem statement at the right level of abstraction  | Issue trees or first principles  |
| Issue trees | Problem is large, tangled, or contains multiple subproblems  | "What are the main independent parts of this problem?"  | A high-level non-overlapping breakdown exists and the major branches are visible  | Prioritization, concept map, or decision matrix  |
| First principles | Assumptions are inherited, weak, or untested  | "What do we know for sure?" and "What are we assuming?"  | Basic truths, constraints, and assumptions are separated  | Issue trees or inversion  |
| Inversion | Risks, anti-goals, or failure modes matter  | "What would make this a terrible solution?"  | Main failure modes and preventable traps are explicit  | Second-order thinking or prioritization  |
| Second-order thinking | Tradeoffs, scaling, or downstream effects matter  | "What happens after the first effect?"  | Short-term benefit and downstream cost are both visible  | Decision matrix or prioritization  |

### Diagnostic extensions

| Tool | Trigger signals | Primary question pattern | Exit condition | Next likely route |
|---|---|---|---|---|
| Iceberg model | User describes visible events or symptoms, but deeper causes are unclear  | "What keeps happening here?" then "What structures might be creating that pattern?"  | The problem is reframed from event level to pattern or structure level  | Ishikawa, concept map, or first principles  |
| Ishikawa | Root-cause diagnosis is needed for a recurring issue  | "What are the main categories of causes behind this problem?"  | Plausible cause categories exist and root-cause branches can be tested  | Issue trees or prioritization  |
| Ladder of inference | The user is jumping from observations to conclusions too quickly  | "What data are we basing that on, and what are we assuming?"  | Data, interpretation, assumptions, and conclusion are separated clearly  | First principles or decision matrix  |
| Concept map | The domain has many entities, actors, dependencies, or relationships  | "What are the key things in this system, and how do they relate?"  | Core entities and links are explicit enough to reason about the system  | Issue trees, iceberg, or decision matrix  |

### Execution choice extensions

| Tool | Trigger signals | Primary question pattern | Exit condition | Next likely route |
|---|---|---|---|---|
| Decision matrix | Several viable options exist and a choice must be made  | "What options do you have, and what factors matter most?"  | Options and evaluation criteria are explicit enough to compare systematically  | Prioritization or spec synthesis  |
| Prioritization | Too many tasks, branches, or ideas compete for attention  | "Which items matter most now, and which create the most impact for the effort?"  | The next small set of actions is ordered clearly  | Spec synthesis  |

## Prioritization default
Use **Eisenhower Matrix** when the main issue is urgency versus importance.

Use **Impact-Effort Matrix** when the main issue is choosing among backlog items, features, or implementation candidates with different payoff and cost profiles.

## Default flow
A practical default sequence for the skill is: start with abstraction laddering when the problem is fuzzy, move to issue trees once the problem is framed, branch into diagnostics only when symptoms or reasoning problems appear, and end with decision matrix or prioritization when choices or sequencing are required.

## Implementation schema
The router can be represented in a compact schema such as:

- `signal_patterns`
- `disqualifiers`
- `opening_question`
- `followups`
- `exit_condition`
- `handoff_rules`

## Working principles
The user should never need to know which mental model is being applied.

The skill should ask targeted questions that progressively clarify the problem while minimizing cognitive load.

The goal is to produce a usable spec containing the problem statement, goals, constraints, user stories, edge cases, and a ready-to-run implementation prompt.

## Spec readiness check
Before the skill emits the final implementation prompt, it must run a mandatory readiness check.

The skill should not treat a spec as complete just because it has a problem statement, user stories, and a plausible solution outline.

A spec is only ready when implementation-critical ambiguity has either been resolved or explicitly recorded as an open decision with a default recommendation.

### Readiness rule
The skill must pause final synthesis and continue discovery if any of the following are missing, contradictory, or under-specified.

| Area | What must be clear before prompt generation |
|---|---|
| Problem framing | The problem statement, target user, and desired outcome are specific and internally consistent  |
| Goals and scope | Goals, non-goals, and V1 boundaries are explicit  |
| Core flow | The main user flow is sequential enough that implementation can begin without inventing product behavior  |
| Inputs and controls | Input methods, control rules, and any modality restrictions are decided or explicitly marked  |
| State and persistence | What state must persist, where it is stored, and what resume behavior means are clear  |
| Completion logic | Success conditions, thresholds, and transitions between states are defined  |
| Edge cases | Important failure cases or UX ambiguities are either resolved or listed as open decisions  |
| Technical assumptions | Major platform or framework assumptions are named when they affect implementation  |

### Closure gate
Before emitting the final implementation prompt, the skill should ask itself these questions.

- Are there unresolved choices that would force the implementation agent to guess? 
- Are any requirements contradictory, such as two different input assumptions or incompatible persistence expectations? 
- Does the prompt require technical choices that were never surfaced during discovery? 
- Would two competent developers build materially different V1 apps from this spec? If yes, continue discovery. 

If the answer to any of these is yes, the skill should not generate the final prompt yet.

Instead, it should either ask a targeted follow-up question or move into a decision-making route such as decision matrix or prioritization to close the gap.

### Open-decision handling
When the user does not want to decide every detail immediately, the skill may proceed only if each unresolved item is captured in this format.

- **Open decision**: the unresolved question.
- **Why it matters**: what part of the implementation it changes.
- **Default recommendation**: the suggested V1 choice.
- **Deferred risk**: what could need revision later.

This allows the skill to preserve momentum without pretending the spec is more settled than it is.

### Prompt emission rule
The final implementation prompt should be generated only when one of the following is true.

- All readiness areas are clear enough for implementation. 
- Remaining ambiguity is explicitly documented as open decisions with defaults. 

If neither condition is met, the skill remains in discovery mode.
