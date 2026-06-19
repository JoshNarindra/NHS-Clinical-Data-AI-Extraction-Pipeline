# v10 — Multimodal Fusion

## Approach
The most complete attempt at docx-based extraction. Combined full table traversal with paragraph text processing in a single pass, capturing both the structured table data and the contextual prose (meeting date headers, preamble text) that sits between tables in the source document.

## Implementation
The document body was iterated in order. Tables were processed row by row with row-context attached. Paragraphs between tables were captured as contextual packages — primarily useful for extracting MDT meeting dates which appeared as bold paragraph headers rather than inside tables. All extracted data was packaged into a unified structure before mapping to the Excel schema.

## Shortcomings
The most thorough implementation of the docx-parsing family, but also the clearest demonstration of its ceiling. Capturing paragraph context helped with meeting dates but did not address spatial awareness, temporal disambiguation, or clinical shorthand variability. After ten iterations, the conclusion was that the root problem was not the extraction strategy but the input format: feeding plain text to a mapper loses the visual context that makes the form interpretable. This realisation motivated the shift to vision-based extraction in v11.
