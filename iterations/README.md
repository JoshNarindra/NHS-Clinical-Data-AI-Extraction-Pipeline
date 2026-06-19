# Iteration History

Each folder represents a distinct architectural approach tried during development. The pipeline evolved from basic regex extraction to a two-stage vision + LLM reasoning system.

| Version | Approach | Result |
|---|---|---|
| v0-OCR-NER-Baseline | Text extraction → MedSPaCy clinical NER → Excel | 127 cells |
| v1-Diamond-Longitudinal | Exhaustive regex mining + sentence-embedding mapping + longitudinal patient linking | 1,102 cells (high recall, temporal misallocation risk) |
| v2-Obsidian-State-Machine | Clinical state machine routing data to phase-specific columns (Baseline / Restaging / Surgical) | 551 cells (clinically safer, too strict) |
| v3-Grid-Coordinate | Fixed table coordinate extraction — Row 7 always = Outcome | 283 cells (brittle, discarded) |
| v4-Semantic-Master | Recursive harvester + low-threshold semantic mapping + additive longitudinal merge | 1,032 cells, exactly 50 rows, zero label bleed |
| v5-Gemini-Harvester | Gemini API journey harvesting — element-by-element doc traversal grouped by patient and MDT date | Exploratory |
| v6-Spatial-Reasoning | Each cell extracted as a spatial node with (row, col) coordinates | Exploratory |
| v7-Contextual-Linker | Row index mapped to functional blocks (Demographics / Staging / Endoscopy / Outcome) | Exploratory |
| v8-Semantic-Grid | Autonomous grid walk with full row context per cell | Exploratory |
| v9-Holistic-Attention | Row header pre-capture before value extraction | Exploratory |
| v10-Multimodal-Fusion | Full table body + paragraph traversal in a single pass | Exploratory |
| v11-VLM-Direct-Logic | Word doc pages rendered as images → Claude vision → structured JSON per case → Excel | Architecture pivot |
| v12-VLM-Reasoning-Auditor | v11 + LLM clinical reasoning auditor: cross-references timeline, routes data to correct columns, flags inferred cells | **Final pipeline** |

The `prompts/` folder contains the prompt evolution that drove each architectural shift.
