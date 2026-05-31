---
name: contour-mind
description: Turn messy software ideas into implementation-ready specs using invisible mental-model routing.
---

# Contour Mind

Use this skill when the user wants to turn a vague software or product idea into a clear, actionable spec.

## Usage
Use this skill before implementation begins, then hand the resulting spec to Codex for execution.

## Job to do
- Diagnose the problem shape.
- Choose the smallest useful mental model.
- Ask one targeted question at a time.
- Keep the models invisible to the user.
- Stop only when the spec is ready or remaining ambiguity is explicitly captured.

## Output
Produce a markdown spec with:
- Problem statement
- Goals and non-goals
- Users
- Core flow
- State model
- Inputs and controls
- Technical assumptions
- Open decisions
- Edge cases
- Codex prompt

## Router
### Core decomposition
- Abstraction laddering
- Issue trees
- First principles
- Inversion
- Second-order thinking

### Diagnostic extensions
- Iceberg
- Ishikawa
- Ladder of inference
- Concept map

### Execution choice extensions
- Decision matrix
- Prioritization

## Default route
1. If the problem is vague or misframed, use abstraction laddering.
2. If the problem is large and splittable, use issue trees.
3. If assumptions are weak, use first principles.
4. If failure modes matter, use inversion.
5. If long-term effects matter, use second-order thinking.
6. If symptoms obscure causes, use iceberg or Ishikawa.
7. If reasoning leaps are happening, use ladder of inference.
8. If system relationships are unclear, use concept map.
9. If options must be chosen, use decision matrix.
10. If too many tasks compete, use prioritization.

## Readiness gate
Do not emit the final prompt until implementation-critical ambiguity is resolved or captured as open decisions with defaults.

## Verification handoff
At the end of the spec, run the contour-verify checklist before finalizing the prompt.
