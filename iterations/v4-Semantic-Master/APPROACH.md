# v4 — Semantic Master

## Approach
Combined the high recall of v1 with precision improvements: eliminate label bleed (column headers being extracted as values) and replace overwriting with an additive longitudinal merge that fills follow-up columns in sequence.

## Implementation
A recursive harvester treated every text fragment as a potential clinical marker. Sentence embeddings at a low similarity threshold (0.2) mapped fragments to Excel columns. Sequential drift filled follow-up columns in order so a patient's second MDT event populated second-MDT columns rather than overwriting the first. Semantic sanitisation stripped column header text from extracted values. Deterministic identity resolver enforced exactly one row per patient.

## Shortcomings
**1,032 cells, exactly 50 rows, zero label bleed** — the best docx-based result. However temporal ordering was still heuristic (first-seen = baseline, second-seen = follow-up) rather than clinically reasoned. Marked as the ceiling of this architecture and closed as a branch.
