# Pipeline Evolution

The pipeline went through 13 iterations across two architectural phases. Each iteration folder contains a full `APPROACH.md` with implementation detail and shortcomings.

---

## Phase 1 — docx Text Parsing (v0–v10)

All early iterations parsed the Word document programmatically using `python-docx`, progressively adding sophistication to extraction and mapping logic.

| Version | Approach | Result |
|---|---|---|
| [v0](../iterations/v0-OCR-NER-Baseline/APPROACH.md) | MedSPaCy clinical NER | 127 cells |
| [v1](../iterations/v1-Diamond-Longitudinal/APPROACH.md) | Regex + sentence-embedding mapping + longitudinal patient linking | 1,102 cells |
| [v2](../iterations/v2-Obsidian-State-Machine/APPROACH.md) | Clinical state machine routing data to phase-specific columns | 551 cells |
| [v3](../iterations/v3-Grid-Coordinate/APPROACH.md) | Fixed table coordinate extraction | 283 cells — discarded |
| [v4](../iterations/v4-Semantic-Master/APPROACH.md) | Recursive harvester + low-threshold embeddings + additive longitudinal merge | 1,032 cells |
| [v5](../iterations/v5-Gemini-Harvester/APPROACH.md) | Gemini API element-by-element journey harvesting | Exploratory |
| [v6](../iterations/v6-Spatial-Reasoning/APPROACH.md) | Spatial nodes with (row, col) coordinates | Exploratory |
| [v7](../iterations/v7-Contextual-Linker/APPROACH.md) | Functional block routing (Demographics / Staging / Endoscopy / Outcome) | Exploratory |
| [v8](../iterations/v8-Semantic-Grid/APPROACH.md) | Full row context per cell for semantic linking | Exploratory |
| [v9](../iterations/v9-Holistic-Attention/APPROACH.md) | Row header pre-capture before value extraction | Exploratory |
| [v10](../iterations/v10-Multimodal-Fusion/APPROACH.md) | Full table + paragraph body traversal in one pass | Exploratory |

The common thread: all ten tried to teach a program to read a clinical form by processing its text. What was missing was the ability to *see* it.

---

## Phase 2 — Vision + Reasoning (v11–v12)

**[v11](../iterations/v11-VLM-Direct-Logic/APPROACH.md)** rendered each case as an image and passed it to Claude vision. The model read the form visually, resolving the spatial awareness problem. A deterministic mapper wrote the extracted JSON to Excel — but with no clinical reasoning, it couldn't handle longitudinal disambiguation.

**[v12](../iterations/v12-VLM-Reasoning-Auditor/APPROACH.md)** added a second LLM pass as a Clinical Reasoning Auditor, reasoning over the patient timeline to route data to the correct longitudinal columns. This is the final pipeline.
