# v1 — Diamond Longitudinal

## Approach
First major architectural step. Exhaustively mine every clinical marker from each proforma using regex, use sentence embeddings to map extracted values to the closest matching Excel column, then link multiple proformas for the same patient into a single longitudinal row.

## Implementation
Regex patterns extracted T/N/M staging, dates, and clinical terms across all table rows. `all-MiniLM-L6-v2` sentence embeddings mapped each extracted value to the 88-column schema by semantic similarity. A longitudinal linker aggregated all cases by NHS number using a "greedy first-found" merge — the first value seen for any field won.

## Shortcomings
**1,102 cells extracted** — a significant breakthrough in coverage. However the greedy merge caused temporal misallocation: if a patient was discussed at baseline and again after chemotherapy, the post-treatment T-stage could overwrite the baseline T-stage. The pipeline had no concept of which event a value belonged to. Evidence-anchored Excel comments provided auditability but did not fix the underlying ordering problem.
