# v5 — Gemini Harvester

## Approach
Shifted extraction to a large language model (Gemini API) rather than rule-based or embedding-based methods. The key insight was to traverse the document element-by-element — tables and paragraphs in the order they appear — rather than extracting all text and then trying to map it.

## Implementation
The document body was iterated sequentially. Paragraphs (typically containing the MDT meeting date as a bold header) were captured for temporal context. Tables were processed row by row. Data was grouped into a per-patient "journey store" keyed by NHS number and MDT date, preserving the chronological structure of the source document.

## Shortcomings
Exploratory — no definitive cell count recorded. Demonstrated that API-based extraction was viable and that preserving document order was important. However the pipeline was still reading docx text sequentially, so the same spatial and context-loss limitations applied. The LLM was given raw text strings, not the visual form, so it had no more spatial awareness than the regex approaches. Set the stage for the shift to vision-based extraction.
