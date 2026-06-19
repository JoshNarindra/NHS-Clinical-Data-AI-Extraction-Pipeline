# v8 — Semantic Grid

## Approach
Every cell extraction carries the full concatenated text of its row as context. Rather than treating each cell in isolation, the extractor sees the entire row — making it easier to resolve which column a value belongs to when multiple clinical concepts appear together.

## Implementation
Each row pre-processed into a `row_text` string (unique cell texts joined with " | "). This string passed alongside the individual cell value to the semantic linker, which used both signals to determine the best matching Excel column via embedding similarity.

## Shortcomings
Row context improved column disambiguation in well-structured rows. But if a row contained values for multiple columns, all values competed against the same context, sometimes pulling semantically similar values to the same column. Still no longitudinal reasoning.
