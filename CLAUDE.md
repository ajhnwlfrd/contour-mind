# Contour Mind

This repo is about building a Claude skill system that turns messy software ideas into clear implementation specs.

## Project shape
- `README.md` explains the project.
- `CLAUDE.md` gives Claude stable repo context.
- `.claude/skills/contour-mind/SKILL.md` contains the discovery workflow.
- `.claude/skills/contour-verify/SKILL.md` contains the verification workflow.
- `references/` holds longer notes and supporting material.
- `examples/` holds sample inputs and outputs.

## Behavior
- Prefer clarity over cleverness.
- Ask targeted questions before assuming requirements.
- Keep the user’s cognitive load low.
- Do not jump to implementation before the spec is ready.
- If something is ambiguous, capture it as an open decision with a default recommendation.

## When to use the skills
Use `contour-mind` when the user wants to turn a vague software idea into a structured spec.
Use `contour-verify` when you want to review that spec for completeness and buildability.
