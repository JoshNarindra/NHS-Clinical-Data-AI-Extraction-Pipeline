# v8 — Semantic Grid

## Approach
Every cell extraction carries the full concatenated text of its entire row as context. Rather than treating each cell in isolation, the extractor sees "Sigmoid adenocarcinoma | T3 N1 M0 | mrT3 mrN1" as the context for the individual value "T3" — making it easier to resolve which column a value belongs to.

## Implementation
Each row was pre-processed into a `row_text` string (unique cell texts joined with " | "). This string was passed alongside the individual cell value to the semantic linker, which used both to determine the best matching Excel column via embedding similarity.

## Shortcomings
Row context improved column disambiguation in cases where multiple staging codes appeared in the same row. However it introduced a new problem: if a row contained values for multiple columns, all values competed against the same row_text context, sometimes pulling semantically similar values to the same column rather than distributing them correctly. Also remained blind to timeline context.
