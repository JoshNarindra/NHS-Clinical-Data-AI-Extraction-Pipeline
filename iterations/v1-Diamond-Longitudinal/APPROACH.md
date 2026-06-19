# v1 — Diamond Longitudinal

## Approach
Exhaustively mine every clinical marker from each proforma using regex, use sentence embeddings to map extracted values to the closest matching Excel column, then link multiple proformas for the same patient into a single longitudinal row.

## Implementation
Regex patterns extracted T/N/M staging, dates, and clinical terms across all table rows. `all-MiniLM-L6-v2` sentence embeddings mapped each extracted value to the 88-column schema by semantic similarity. A longitudinal linker aggregated all cases by NHS number using a "greedy first-found" merge. Evidence snippets were injected as Excel cell comments for auditability.

## Shortcomings
**1,102 cells extracted** — first major coverage breakthrough. However the greedy merge caused temporal misallocation: a post-treatment T-stage could overwrite the baseline T-stage because the pipeline had no concept of which event in the patient's timeline a value belonged to.
