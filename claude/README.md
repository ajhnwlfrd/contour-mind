# Contour Mind (Claude Skill)

This is a Claude skill that helps software developers move from vague ideas to clear, implementation‑ready specifications. It acts as a thinking layer — a guided discovery process that runs before coding.

The core belief is simple: better software starts before coding. A coding agent can only execute well when the task is clear, bounded, and testable, so Contour Mind exists to produce that clarity.

## What it is not

Contour Mind is not an implementation agent, a project management tool, or a replacement for product judgment. It is optimized for feature‑level discovery and spec shaping, not long‑horizon planning.

## How to run it

### Prerequisites

- [Claude Code](https://claude.ai/code) — the CLI tool. Skills are slash commands that Claude Code loads automatically from `.claude/skills/` when you open a project folder.

### Run steps

1. Download or clone this repository.
2. Open a terminal or command prompt in the project folder.
3. Run `claude` — Claude Code automatically loads the skills from `.claude/skills/`.
4. Type `/contour-mind` and press Enter to start.

### First run example

Try a rough idea like: “I want to build a bulk invoice upload flow for our back office.” Contour Mind should turn that into a structured spec and a scoped Claude Code prompt.

## Why it exists

Most AI‑assisted development tools focus on code generation: they help you build once you already know what to build. Contour Mind focuses on the step before that.

It helps you clarify what problem is actually being solved, who is affected, what assumptions you are making, what edge cases could break the solution, and where the MVP boundary should sit. The result is a structured spec and a scoped Claude Code prompt — not just more questions.

## How it works

Contour Mind uses an adaptive router that quietly selects the right thinking frame based on the shape of the conversation. You do not need to know the names of any frameworks.

- If the idea is vague, it clarifies the level of abstraction.  
- If the problem is large, it decomposes it into branches.  
- If assumptions are weak, it tests them.  
- If failure matters, it explores what could go wrong.  
- If too many tasks compete, it helps you prioritize.

Internally, it draws on models like abstraction laddering, issue trees, first principles, inversion, and second‑order thinking, but you experience it as a natural conversation.

## Workflow

Use Contour Mind to shape the spec, then Contour Verify to harden it before implementation.

1. Run `contour-mind` to turn a rough idea into a structured spec.
2. Run `contour-verify` to check the spec for completeness and buildability.
3. Revise the spec before handing it to an implementation agent.

## Verification checks

`contour-verify` checks the spec for:

- Missing goals or non‑goals.  
- Ambiguous requirements.  
- Missing edge cases or failure modes.  
- Conflicting constraints.  
- Unclear MVP scope.  
- Weak testability or implementation readiness.

## Output

The thinking layer produces two artifacts:

- **Structured markdown spec** — covers problem statement, goals, non‑goals, user stories, edge cases, failure modes, constraints, MVP scope, and success criteria.  
- **Claude Code prompt** — a scoped, actionable implementation prompt derived directly from the spec.

## Example

Rough idea: “I want a bulk invoice upload flow.”

Contour Mind helps turn that into a problem statement, a decomposed workflow, key assumptions, edge cases like invalid files or duplicate uploads, and a scoped prompt for implementing the first slice.

## How the router thinks

The router chooses the smallest useful thinking frame for the current moment.

- Use abstraction laddering when the idea is vague or overly solution‑shaped.  
- Use issue trees when the problem can be split into independent branches.  
- Use first principles when assumptions are weak or uncertain.  
- Use inversion when failure modes matter.  
- Use second‑order thinking when downstream effects matter.  
- Use decision support when multiple options or competing tasks need ordering.

If no single frame fits, the router can blend or sequence frames over the conversation.

## What’s in this repo

- `.claude/skills/contour-mind/SKILL.md` — discovery and spec generation workflow.  
- `.claude/skills/contour-verify/SKILL.md` — spec review and validation workflow.  
- `../docs/paper.md` — design paper covering the router, mental models, and future work.  
- `../references/router-spec.md` — detailed router spec with routing tables, readiness checks, and closure gate.  
- `../examples/` — sample inputs and outputs.

See `../docs/paper.md` for a deeper dive into the router design and thinking frames.