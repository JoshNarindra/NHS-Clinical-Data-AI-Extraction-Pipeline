# v7 — Contextual Linker

## Approach
Rather than using exact row coordinates (which break on merged cells), map row index to a broader clinical functional block. Rows 0–1 are always Demographics, rows 2–3 are Staging/Diagnosis, rows 4–5 are Endoscopy, and rows 6+ are the Outcome Narrative. Extraction logic is applied per block rather than per coordinate.

## Implementation
A `get_functional_block()` function mapped each row index to one of four zones. Extraction rules were written per block — demographics rules only fired in the Demographics zone, staging rules only fired in the Staging zone — reducing the risk of staging codes being extracted from the wrong clinical section.

## Shortcomings
More robust than v6 against single merged-cell shifts (the blocks are wide enough to absorb minor row drift), but still vulnerable to larger structural variations. Also did not solve the longitudinal problem — the functional blocks helped with field-level extraction within a single proforma but had no mechanism for understanding where in a patient's timeline that proforma sat.
