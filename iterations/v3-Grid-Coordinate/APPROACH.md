# v3 — Grid Coordinate

## Approach
Hypothesis: MDT proformas are standardised enough that every case follows an identical visual grid. If Row 7 always contains the MDT outcome, extraction becomes a simple coordinate lookup rather than an NLP problem.

## Implementation
Each table cell was addressed by its (row, col) index. A fixed mapping translated coordinates directly to Excel columns — no regex, no embeddings, no NER.

## Shortcomings
**283 cells — discarded.** The assumption was wrong. Merged cells shift every subsequent row index, breaking all downstream mappings for that case. Worked on a small well-formatted sample but failed at scale. Confirmed that position cannot be the primary extraction signal in real-world clinical documents.
