# v7 — Contextual Linker

## Approach
Map row index to broad clinical functional blocks rather than exact coordinates. Rows 0–1 = Demographics, 2–3 = Staging, 4–5 = Endoscopy, 6+ = Outcome. Apply extraction rules per block rather than per coordinate, absorbing minor row shifts.

## Implementation
`get_functional_block()` mapped each row index to one of four zones. Extraction rules only fired within the appropriate zone — staging rules ignored rows in the Demographics block — reducing false positives.

## Shortcomings
More robust than v6 against single merged-cell shifts but still vulnerable to larger structural variations. Blocks help with field-level extraction within a single proforma but have no mechanism for understanding where in the patient's longitudinal timeline that proforma sits.
