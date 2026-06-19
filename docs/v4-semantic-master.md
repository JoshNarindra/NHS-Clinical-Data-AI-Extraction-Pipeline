# v4 — Semantic Master

## Approach
Combined the high recall of v1 with precision improvements to address its two main failures: label bleed (where column headers were being extracted as values) and temporal overwriting (where later values replaced earlier ones rather than filling the correct follow-up column).

## Implementation
A recursive harvester treated every text fragment as a potential clinical marker. Sentence embeddings mapped each fragment to the closest Excel column at a low similarity threshold (0.2) to maximise recall. An additive longitudinal linker used "sequential drift" — filling follow-up columns in order rather than overwriting — so a patient's second MDT event populated the second-MDT columns rather than clobbering the first. Semantic sanitisation stripped column header text from extracted values to eliminate label bleed. A deterministic identity resolver ensured exactly one row per patient.

## Shortcomings
**1,032 cells, exactly 50 rows, zero label bleed.** The cleanest docx-based result. However the temporal problem was addressed with heuristics (sequential order) rather than genuine reasoning — the pipeline assumed the first-seen value was baseline and the second was follow-up, which worked most of the time but was not clinically defensible. Marked as the ceiling of what was achievable with this architecture and closed as a branch.
