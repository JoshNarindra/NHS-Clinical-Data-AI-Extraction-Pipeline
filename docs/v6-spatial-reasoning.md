# v6 — Spatial Reasoning

## Approach
Rather than extracting raw text and losing position, wrap every extracted cell in a "Spatial Node" object carrying its table row and column coordinates. Clinical meaning would be inferred from position — a cell in column 0 is typically a label; a cell in column 1 is typically a value.

## Implementation
Each table cell was packaged as a node containing: the cell text, its (row, col) index, the NHS number of the case, and the full row text as context. A reasoner then used positional rules to classify each node as a label, a value, or a staging code.

## Shortcomings
Hit the same wall as v3. Merged cells in Word documents cause column indices to be unreliable — a two-column merged cell reports as a single cell at column 0, shifting every subsequent cell's index. Position can be a useful secondary signal when it holds, but it cannot be the primary extraction mechanism for documents that were not generated from a strict template.
