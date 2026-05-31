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

## Sample output

**Spec reviewed:** Bulk invoice upload flow.

**Result:** Needs revision.

**Issues found:**
- Resume behaviour after a partial upload failure is not defined.
- Success criteria does not specify what happens to rejected rows after submission.
- Permission model is missing — who can upload, and can they see each other's uploads?

**Suggested fixes:** Add an open decision for partial failure handling with a default. Clarify whether rejected rows are discarded or held for retry. Add a permissions constraint to the non-goals or requirements.

**Ready for implementation:** No — resolve or capture the three issues above as open decisions before handing to a coding agent.
