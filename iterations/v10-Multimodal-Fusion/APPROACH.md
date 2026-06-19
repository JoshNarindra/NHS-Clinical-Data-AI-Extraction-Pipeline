# v10 — Multimodal Fusion

## Approach
The most complete docx-based attempt. Combined full table traversal with paragraph text processing in a single pass — capturing both structured table data and contextual prose (meeting date headers) that sits between tables in the source document.

## Implementation
Document body iterated in order. Tables processed row by row with row context attached. Paragraphs between tables captured as contextual packages, primarily useful for MDT meeting dates which appeared as bold paragraph headers rather than inside tables.

## Shortcomings
The ceiling of the docx-parsing family. Capturing paragraph context helped with meeting dates but did not address spatial awareness, temporal disambiguation, or clinical shorthand variability. After ten iterations the conclusion was clear: the root problem was not the extraction strategy but the input format — feeding plain text to a mapper loses the visual context that makes the form interpretable. This motivated the shift to vision-based extraction in v11.
