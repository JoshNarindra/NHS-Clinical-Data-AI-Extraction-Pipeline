# v2 — Obsidian State Machine

## Approach
Directly addressed v1's temporal misallocation problem. Each proforma was classified into a clinical phase — Baseline, Restaging, or Surgical — and a routing table directed extracted values to phase-specific Excel columns rather than overwriting a single shared field.

## Implementation
A keyword-based phase classifier scanned the document header and outcome text for signals (e.g. "neoadjuvant", "post-CRT", "surgical review") to assign a phase label. The routing table then mapped phase + field type to the correct column (e.g. a T-stage in a Restaging document routes to "2nd MRI: mrT" rather than "Baseline MRI: mrT").

## Shortcomings
**551 cells extracted.** Clinically the safest iteration — no temporal misallocation — but coverage dropped sharply from v1. Real clinical shorthand is inconsistent: phrases that signal a restaging event in one proforma appear differently in another, or not at all. The classifier missed enough phase signals that large amounts of data were left unrouted rather than risking a wrong assignment. Highlighted the fundamental tension between safety and coverage.
