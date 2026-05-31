# Full Example: Bulk Invoice Upload Flow

This example shows the complete Contour Mind workflow — from rough idea to verified spec to implementation prompt.

---

## Input

> I need to build a bulk invoice upload flow for our back office. Right now, the finance team receives invoice data in CSV files and enters it manually into the system. This is slow, repetitive, and prone to errors. I want a workflow where a user can upload a CSV, review any validation issues, fix problems before submission, and then send the valid invoices into the system.
>
> I want the first version to focus on the minimum viable flow, but I also want to capture edge cases like duplicate invoices, missing fields, invalid currency values, and partial upload failures. I'm not sure yet whether we should support PDF matching in V1, so treat that as an open decision unless it clearly belongs in the MVP.

---

## Discovery (abbreviated)

Contour Mind diagnosed the idea as large and splittable, opened with issue-tree decomposition, then used inversion to surface failure modes.

**Contour Mind:** What decision does the upload workflow need to support — are finance users correcting and submitting themselves, or uploading for someone else to approve?

**User:** They upload and submit themselves. No approval step in V1.

**Contour Mind:** When a row fails validation, can users fix it inline in the browser, or do they re-upload the corrected CSV?

**User:** Inline correction. We don't want to make them start over.

**Contour Mind:** What receives valid invoices on the backend — REST API, direct database write, or something else?

**User:** REST API. POST to `/api/invoices/batch`.

---

## Generated spec

### Title
Bulk Invoice Upload — V1

### Problem statement
Finance staff currently enter invoice data from CSV files manually, one row at a time. This is slow, error-prone, and creates a backlog when volume spikes. A bulk upload flow will allow any finance user to upload a CSV, review validation issues, correct errors inline, and submit valid invoices to the billing system in a single session.

### Goals
- Accept CSV file uploads up to 10MB.
- Parse and validate each row against required fields.
- Display a row-level error table for invalid entries with inline correction.
- Submit all valid rows to the billing system via `POST /api/invoices/batch`.
- Return a submission summary showing accepted and rejected counts.

### Non-goals
- PDF invoice matching — out of scope for V1.
- Multi-currency conversion — out of scope.
- Batch scheduling or deferred submission — out of scope.
- Approval workflows — out of scope.

### Users
Finance staff with back-office system access. No external users.

### Core flow
1. User navigates to the bulk upload page.
2. User selects or drags a CSV file.
3. System parses the file and validates each row.
4. If errors exist, the system shows a row-level error table with editable fields.
5. User corrects errors inline and clicks Re-validate.
6. Once all rows are valid, the Submit button becomes active.
7. User submits. System POSTs valid rows to `/api/invoices/batch`.
8. System displays a summary: X accepted, Y rejected.

### State model
| State | Description |
|---|---|
| `idle` | No file selected. |
| `parsing` | File uploaded, parsing in progress. |
| `validation_failed` | One or more rows have errors. |
| `ready` | All rows valid, submission enabled. |
| `submitting` | POST in progress. |
| `done` | Submission complete, summary shown. |

### Required CSV fields
| Field | Type | Validation rule |
|---|---|---|
| invoice_number | string | Required, unique within file |
| amount | decimal | Required, greater than 0 |
| currency | string | Required, ISO 4217 code (e.g. GBP, USD) |
| due_date | date | Required, YYYY-MM-DD format |
| supplier_id | string | Required |

### Inputs and controls
- **File input:** CSV only, max 10MB. Reject other formats with a clear message before parsing.
- **Error table:** Editable cells for invalid fields only. Valid fields in each row are read-only.
- **Re-validate button:** Re-runs validation on corrected rows without re-uploading the file.
- **Submit button:** Disabled until all rows pass validation.

### Technical assumptions
- The billing system REST API is available and accepts `POST /api/invoices/batch` with a JSON array.
- Authentication is handled upstream — the upload page is behind the existing auth layer.
- CSV files use UTF-8 encoding.
- No mobile-specific behaviour is required for V1.

### Edge cases
- File exceeds 10MB: reject immediately before parsing, show size error.
- File is not a CSV: reject with a format error.
- All rows are invalid: show the full error table, block submission entirely.
- Duplicate `invoice_number` within the same file: flag both rows as errors.
- `invoice_number` already exists in the billing system: surface as a per-row warning in the submission summary, not during validation.
- API returns partial success: show per-row outcome in the summary.
- API unavailable during submission: show a network error with a retry option.
- User navigates away with unsaved inline corrections: warn before leaving.

### Open decisions
| Decision | Default recommendation | Deferred risk |
|---|---|---|
| PDF matching in V1? | No — revisit for V2 if finance team requests it. | PDF matching may require a separate ingestion pipeline. |
| Should rejected rows persist after submission for retry? | No — user corrects in a new session. | High rejection rates may frustrate users if corrections are lost. |

### Success criteria
- A finance user can upload a 500-row CSV, fix validation errors inline, and submit without engineering involvement.
- Submission response time is under 5 seconds for files up to 500 rows.
- All invalid rows are identified before submission — no silent failures.

---

## Verification result (contour-verify)

**Result:** Pass.

| Check | Status |
|---|---|
| Problem framing is specific | ✓ |
| Goals and non-goals are non-overlapping | ✓ |
| Core flow is sequential and implementable | ✓ |
| State model is complete | ✓ |
| Inputs and controls are unambiguous | ✓ |
| Technical assumptions are named and plausible | ✓ |
| Open decisions are captured with defaults | ✓ |
| Edge cases are realistic | ✓ |
| Implementation prompt is actionable | ✓ |

**Notes:** The open decision on rejected row persistence is well-captured. If the finance team later requests retry behaviour, raise a new spec slice rather than expanding V1 scope.

**Ready for implementation:** Yes.

---

## Implementation prompt

Implement a bulk invoice upload flow for the back-office finance portal.

**Task:** Build the upload, validation, inline correction, and submission workflow as a single-page flow.

**Scope:**
- File input: accept CSV only, reject files over 10MB before parsing.
- CSV parser: validate each row against the required fields table above (invoice_number, amount, currency, due_date, supplier_id).
- Validation error table: editable cells for failed fields only; valid fields are read-only per row. Include a Re-validate button that re-checks corrected rows without re-uploading.
- Submit button: disabled until all rows pass validation. On click, POST the validated row array to `/api/invoices/batch` as a JSON array.
- Submission summary: show accepted count, rejected count, and per-row status for any rows the API rejects.

**Edge cases to handle:**
- File format or size rejection before parsing.
- Duplicate `invoice_number` within the same upload — flag both rows.
- API unavailable: show error with a retry option.
- Partial API success: surface per-row outcome in the summary.
- Navigation warning if the user leaves with unsaved inline corrections.

**Do not change:** The existing auth layer, the billing system API contract, or any other upload flows in the portal.

**Definition of done:** A finance user can complete the full upload → validate → correct → submit cycle in a single session, without a page reload or engineering support.
