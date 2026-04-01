# Copilot Instructions

## Host Integration Map (`host-integration` issues)

When assigned to an issue labeled `host-integration`, follow these instructions.

You are responsible for updating the file:

`a_appendix/host-integration-map.md`

Your job starts from a GitHub issue created from the "Host integration update" form.

When handling an issue:

1. Extract:
   - Host product
   - Host product version
   - Dynamo version
   - Source/evidence
   - Notes

2. Update only the relevant product section in:
   `a_appendix/host-integration-map.md`

3. Follow these rules strictly:
   - Preserve the existing markdown table style.
   - Insert the new row in descending version order within the correct product section.
   - Do not duplicate an existing row.
   - Do not modify unrelated sections.
   - If the product is "Other" and no section exists, stop and explain that a maintainer must define the section first.
   - If the issue data is incomplete or ambiguous, stop and comment with exactly what is missing.

4. After editing:
   - Open a pull request with a concise title:
     `Update host integration map for <Product> <Host Version>`
   - In the PR body, summarize:
     - product
     - host version
     - dynamo version
     - source/evidence
     - whether a new row was added or a duplicate was detected

5. Never invent versions.
6. Never normalize version numbers unless the issue explicitly provides the normalized value.
7. Prefer exact matching to the style already present in the file.
