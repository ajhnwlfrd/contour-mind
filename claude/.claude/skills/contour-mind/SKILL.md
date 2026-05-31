---
name: contour-mind
description: Turn messy software ideas into implementation-ready specs using invisible mental-model routing.
---

# Contour Mind

Use this skill when the user wants to turn a vague software or product idea into a clear, actionable spec.

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
- Claude Code prompt

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

## Sample output

**Input:** "I want to build a bulk invoice upload flow for our back office."

---

**Problem statement:** Finance staff manually enter invoice data from CSV files, causing errors and delays. A bulk upload flow would let users upload, validate, and submit invoices in one workflow.

**Goals:** CSV upload and parsing, field validation with user-facing errors, duplicate detection, submission to the billing system.

**Non-goals:** PDF matching, multi-currency support, and batch scheduling are out of scope for V1.

**Edge cases:** Missing required fields, duplicate invoice numbers, invalid currency codes, partial upload failures, files exceeding size limits.

**Open decisions:**
- Should users correct individual rows inline or re-upload the full file? *Default: inline correction.*

**Success criteria:** A finance user can upload a 500-row CSV, review and fix validation errors, and submit without engineering involvement.

---

**Claude Code prompt:** Implement a CSV upload flow for bulk invoice ingestion. Accept CSV files up to 10MB. Validate each row for required fields (invoice number, amount, currency, due date). Flag duplicates by invoice number. Show a row-level error table for invalid entries with inline correction. On submit, POST valid rows to `/api/invoices/batch`. Return a summary of accepted and rejected rows.
