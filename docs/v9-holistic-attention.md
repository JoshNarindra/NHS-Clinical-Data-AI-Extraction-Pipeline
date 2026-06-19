# v9 — Holistic Attention

## Approach
Before extracting values, pre-scan every row to identify its label — typically the first non-empty cell, which in MDT proformas is usually the field name (e.g. "Histology:", "MDT Outcome:"). Attach this label to every value extracted from that row, giving the extraction step an explicit field-name anchor rather than relying purely on position or embedding similarity.

## Implementation
A row header dictionary was built by scanning each row and recording the first non-empty cell text. This header was then paired with each subsequent value in the row, creating explicit (label, value) pairs. The semantic mapper operated on the label rather than the raw value, making column assignment significantly more reliable for rows where the label unambiguously named the field.

## Shortcomings
Worked well when rows had clear, consistent label cells — which was true for demographics and simple staging rows. Failed when multiple clinical concepts shared a single row (common in narrative outcome text), and when label text varied across cases (e.g. "Endoscopy findings:" vs "Scope findings:" vs no label at all). Still no longitudinal reasoning.
