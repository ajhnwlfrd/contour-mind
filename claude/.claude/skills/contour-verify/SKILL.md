---
name: contour-verify
description: Review a generated software spec for clarity, completeness, consistency, and implementation readiness.
---

# Contour Verify

Use this skill when a spec needs a final quality check before implementation.

## Job to do
- Check the spec for internal consistency.
- Flag missing implementation-critical details.
- Surface contradictions and vague claims.
- Identify areas likely to cause build drift.
- Recommend the smallest useful revision.

## Review checklist
- Problem framing is specific.
- Goals and non-goals are non-overlapping.
- Core flow is sequential and implementable.
- State model is complete.
- Inputs and controls are unambiguous.
- Technical assumptions are named and plausible.
- Open decisions are captured with defaults.
- Edge cases are realistic.
- The Claude Code prompt is actionable.

## Output
Return:
- Pass / needs revision.
- Short list of issues.
- Suggested fixes.
- Whether the spec is ready for implementation.
