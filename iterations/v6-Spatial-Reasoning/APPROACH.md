# v6 — Spatial Reasoning

## Approach
Wrap every extracted cell in a Spatial Node carrying its (row, col) coordinates and full row text as context. Infer clinical meaning from position — column 0 is typically a label, column 1 a value.

## Implementation
Each table cell packaged as a node with coordinates, NHS number, cell text, and row context. A positional reasoner classified each node as a label, value, or staging code based on its column index.

## Shortcomings
Merged cells in Word documents cause column indices to be unreliable — a merged cell reports as column 0, shifting every subsequent cell's index. Position can be a useful secondary signal but cannot be the primary mechanism for documents not generated from a strict template.
