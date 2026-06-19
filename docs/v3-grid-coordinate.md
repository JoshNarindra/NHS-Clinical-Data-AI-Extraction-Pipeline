# v3 — Grid Coordinate

## Approach
Hypothesis: MDT proformas are standardised enough that every case follows an identical visual grid. If Row 7 always contains the MDT outcome, extraction becomes a simple coordinate lookup rather than an NLP problem.

## Implementation
Each table cell was addressed by its (row, col) index. A fixed mapping translated coordinates directly to Excel columns — no regex, no embeddings, no NER. The assumption was that the document structure was deterministic enough to make position a reliable signal.

## Shortcomings
**283 cells extracted — discarded.** The assumption was wrong. Real clinical documents contain merged cells, variable row counts, and layout shifts introduced by different MDT coordinators. A single merged cell shifts every subsequent row index, breaking all downstream mappings for that case. The approach worked on a small sample of well-formatted proformas but failed at scale across the full 50-case dataset. Confirmed that position alone cannot be trusted as a primary signal in real-world clinical documents.
