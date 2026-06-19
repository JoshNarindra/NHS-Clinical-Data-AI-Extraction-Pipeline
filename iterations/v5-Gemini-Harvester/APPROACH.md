# v5 — Gemini Harvester

## Approach
Shifted extraction to a large language model (Gemini API) rather than rules or embeddings. Traversed the document element-by-element — tables and paragraphs in order — to preserve the chronological structure of the source rather than extracting all text as a flat blob.

## Implementation
Document body iterated sequentially. Paragraphs (typically bold MDT date headers) captured for temporal context. Tables processed row by row. Data grouped into a per-patient journey store keyed by NHS number and MDT date.

## Shortcomings
Exploratory — no definitive cell count. Still reading docx text, so the same spatial and context-loss limitations applied. The LLM received raw strings, not the visual form, so it had no more spatial awareness than the regex approaches. Set the stage for the shift to vision-based extraction.
