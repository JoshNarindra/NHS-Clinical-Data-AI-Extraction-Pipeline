# Pipeline Evolution

The pipeline went through 13 iterations across two distinct architectural phases.

---

## Phase 1 — docx Text Parsing (v0–v10)

All early iterations parsed the Word document programmatically using `python-docx`, progressively adding sophistication to the extraction and mapping logic.

| Version | Approach | Result | Why it fell short |
|---|---|---|---|
| v0 | MedSPaCy clinical NER | 127 cells | NER tags entities but can't determine which Excel column they belong to |
| v1 | Exhaustive regex + sentence-embedding mapping + longitudinal patient linking | 1,102 cells | High recall but greedy merge caused temporal misallocation — post-treatment staging overwrote baseline values |
| v2 | Clinical state machine routing data to phase-specific columns | 551 cells | Too strict — real clinical shorthand is inconsistent, most data went unrouted |
| v3 | Fixed table coordinate extraction (Row 7 = Outcome, etc.) | 283 cells | Merged cells shifted row indices, breaking all mappings |
| v4 | Recursive harvester + low-threshold embeddings + additive longitudinal merge | 1,032 cells | Best docx result, but temporal ordering was still heuristic rather than reasoned |
| v5–v10 | Successive experiments: Gemini API harvesting, spatial nodes, functional block routing, row-context embeddings, row-header anchoring, full-body fusion | Exploratory | Each addressed a specific gap but couldn't escape the core limitation: feeding plain text to a mapper loses the 2D visual context that makes the form interpretable |

The common thread across all ten iterations: they tried to teach a program to read a clinical form by processing its text. What was missing was the ability to *see* it.

---

## Phase 2 — Vision + Reasoning (v11–v12)

**v11** rendered each case as an image and passed it to a vision LLM (Claude 3 Haiku). The model read the form visually, resolving the spatial awareness problem immediately. It returned a structured JSON per case, which a deterministic mapper wrote to Excel. The gap that remained: Stage 2 had no clinical reasoning — it couldn't tell a baseline T-stage from a post-treatment one.

**v12** added a second LLM pass as a Clinical Reasoning Auditor. Rather than mapping mechanically, it reasoned over the patient's timeline — distinguishing baseline events from restaging events, routing data to the correct longitudinal columns, and flagging any inferred values with cell comments for clinical review. This became the final pipeline.
