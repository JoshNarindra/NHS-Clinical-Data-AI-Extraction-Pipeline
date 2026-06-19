# v2 — Obsidian State Machine

## Approach
Directly addressed v1's temporal misallocation. Each proforma was classified into a clinical phase — Baseline, Restaging, or Surgical — and a routing table directed extracted values to phase-specific Excel columns.

## Implementation
A keyword-based phase classifier scanned document headers and outcome text for signals (e.g. "neoadjuvant", "post-CRT", "surgical review"). The routing table then mapped phase + field type to the correct column — a T-stage in a Restaging document routes to "2nd MRI: mrT" rather than "Baseline MRI: mrT".

## Shortcomings
**551 cells extracted.** Clinically the safest iteration but coverage dropped sharply. Real clinical shorthand is inconsistent: the same restaging event is described differently across cases, or not signalled at all. The classifier missed enough phase signals that large amounts of data went unrouted rather than risk a wrong assignment.
