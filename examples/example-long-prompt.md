# Example Long prompt

I need to build a bulk invoice upload flow for our back office.

Right now, the finance team receives invoice data in CSV files and enters it manually into the system. This is slow, repetitive, and prone to errors. I want a workflow where a user can upload a CSV, review any validation issues, fix problems before submission, and then send the valid invoices into the system.

Please help me shape this into a clear spec. I want the first version to focus on the minimum viable flow, but I also want to capture edge cases like duplicate invoices, missing fields, invalid currency values, and partial upload failures. I’m not sure yet whether we should support PDF matching in v1, so treat that as an open decision unless it clearly belongs in the MVP.

The output should be a structured spec with:

problem statement.

goals and non-goals.

user flow.

constraints.

edge cases.

open decisions.

success criteria.

Then generate a scoped implementation prompt for the first build slice.
