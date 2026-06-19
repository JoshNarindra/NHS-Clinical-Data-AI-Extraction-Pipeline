# v9 — Holistic Attention

## Approach
Pre-scan every row to identify its label — typically the first non-empty cell, which in MDT proformas is usually the field name ("Histology:", "MDT Outcome:"). Attach this label to every value extracted from that row, giving the semantic mapper an explicit field-name anchor.

## Implementation
A row header dictionary built by scanning each row and recording the first non-empty cell. Each (label, value) pair passed to the semantic mapper, which operated on the label rather than the raw value for column assignment.

## Shortcomings
Worked well when rows had clear, consistent label cells. Failed when multiple clinical concepts shared a single row (common in narrative outcome text) and when label phrasing varied across cases ("Endoscopy findings:" vs "Scope findings:" vs no label at all). No longitudinal reasoning.
