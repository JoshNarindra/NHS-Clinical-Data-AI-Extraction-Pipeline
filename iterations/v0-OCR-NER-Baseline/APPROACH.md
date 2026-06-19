# v0 — OCR-NER Baseline

## Approach
A straightforward three-stage pipeline: extract raw text from the Word document, run clinical named entity recognition to tag medical concepts, then write the results to Excel.

## Implementation
Used `python-docx` to pull text from each case table. MedSPaCy with custom clinical pipes handled entity recognition — identifying TNM staging codes, procedure dates, and diagnosis terms. A simple mapper then wrote tagged entities to the corresponding Excel columns.

## Shortcomings
**127 cells extracted.** NER identifies that "T3" is a staging code but has no way to know which Excel column it belongs to without understanding the surrounding clinical context. No longitudinal linking — each case was treated as an isolated record with no concept of the same patient appearing across multiple proformas. Served as the baseline benchmark.
