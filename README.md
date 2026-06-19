# Clinical AI Extraction Pipeline

An AI pipeline that extracts structured clinical data from NHS Multidisciplinary Team (MDT) meeting documents and populates a longitudinal patient database — turning unstructured Word documents into a searchable, auditable Excel database.

---

## The Problem

NHS oncology MDT meetings produce semi-structured Word documents containing patient histories, staging, imaging results, and treatment decisions. This data is effectively locked — difficult to query, audit, or use for research. Clinicians resort to maintaining manual databases, which is time-consuming and inconsistent.

**The task:** given 50 synthetic MDT proformas, automatically extract and populate an 88-column longitudinal patient database matching a clinician-defined schema.

*Problem statement by Dr Anita Wale, Consultant Radiologist, St George's University Hospitals NHS Foundation Trust. Full brief in [`docs/Problem_Statement.md`](docs/Problem_Statement.md).*

---

## Why Earlier Approaches Failed

The first ten iterations all parsed the Word document programmatically using `python-docx`. This turned out to be the wrong foundation for three interconnected reasons.

**No spatial awareness.** MDT proformas are visually structured forms — a clinician reads them as a 2D layout where position carries meaning. `python-docx` strips that away, returning a flat stream of text. Attempts to recover position using table row/column coordinates broke immediately when real documents had merged cells and non-standard row shifts.

**Temporal misallocation.** A patient discussed at multiple MDT meetings appears in multiple proformas. The 88-column schema is longitudinal — separate columns for a baseline MRI, a restaging MRI after chemotherapy, and a follow-up MRI later. Without understanding clinical context, the pipeline would confidently place a post-treatment T-stage into the baseline column. v1 achieved 1,102 extracted cells but with significant temporal errors. v2 introduced a clinical state machine to route data to the correct phase — it was safer but too rigid for the inconsistent shorthand clinicians actually use, dropping coverage to 551 cells.

**Clinical shorthand variability.** The same concept appears as "CT Abdo/Pelvis", "CT CAP", "CT abdomen", "CT thorax/abdomen/pelvis" across different cases. Regex and NER rules written for one phrasing silently miss the others.

The common thread: these approaches tried to teach a program to read a clinical document the way a clinician does. The v11–v12 architecture reversed the assumption entirely. See [`docs/pipeline-evolution.md`](docs/pipeline-evolution.md) for the full iteration breakdown.

---

## Pipeline Architecture

The final pipeline ([`src/`](src/)) uses two stages:

```
data/hackathon-mdt-outcome-proformas.docx  (50 MDT cases)
        │
        ▼
  [Stage 1: Claude Vision]  ←  src/stage1_claude_vision.py
  Each case rendered as an image and sent to a vision LLM.
  Extracts demographics, staging, imaging, and MDT outcome into structured JSON.
        │
        ▼
  JSON (one file per patient)
        │
        ▼
  [Stage 2: Clinical Reasoning Auditor]  ←  src/stage2_clinical_reasoning_auditor.py
  An LLM acting as a UK NHS colorectal consultant.
  Cross-references the patient timeline to route facts to the correct columns
  (e.g. distinguishes baseline MRI from post-treatment restaging MRI).
  Flags any inferred values with cell comments for human review.
        │
        ▼
  output/final_clinical_database.xlsx  (88 columns, 50 patient rows)
```

---

## Results

Validated against the source Word document across all 50 cases. Full methodology and results in [`validation/`](validation/).

| Group | Accuracy | Coverage |
|---|---|---|
| Demographics (MRN, NHS, DOB, Gender, MDT date) | **97.2%** | **100%** |
| 1st MDT treatment approach | **94.7%** | 76.0% |
| Histology / diagnosis | 36.8% | 71.7% |
| Endoscopy | 80.0% | 15.6% |
| Baseline CT | **100%** | 3.0% |
| Baseline MRI | **100%** | 11.3% |
| **Overall** | **76.6%** | — |

**Key finding:** CT and MRI accuracy is 100% — when imaging values are extracted they are always correct. Coverage gaps trace to Stage 1 not separating imaging data into dedicated JSON fields. This is fixable with a prompt update.

Error attribution: 78% of all errors originate at Stage 1 (vision extraction), 22% at Stage 2 (mapping).

---

## Repository Structure

```
├── src/                    # Final pipeline (v12)
│   ├── split_and_image.py                        # Converts Word doc pages to images
│   ├── stage1_claude_vision.py                   # Vision extraction → JSON
│   ├── stage2_clinical_reasoning_auditor.py      # LLM reasoning → Excel
│   ├── stage2_hybrid_clinical_auditor.py         # Hybrid variant
│   ├── fix_json_logic.py                         # JSON repair utilities
│   └── config.py                                 # API keys and paths
├── data/
│   ├── hackathon-mdt-outcome-proformas.docx      # Input: 50 synthetic MDT cases
│   └── hackathon-database-prototype.xlsx         # Ground truth schema
├── output/
│   └── final_clinical_database.xlsx              # Pipeline output
├── validation/                                   # Validation framework and results
├── docs/
│   ├── Problem_Statement.md                      # Clinical problem brief
│   ├── pipeline-evolution.md                     # All 13 iterations summarised
│   └── *.png                                     # Input/output format examples
└── iterations/                                   # Full code and outputs per iteration
    ├── README.md                                 # Version summary table
    └── v0/ – v12/  (each contains APPROACH.md)
```

---

## Running the Pipeline

```bash
pip install anthropic python-docx openpyxl pandas tqdm

# Set your Anthropic API key in src/config.py

# Step 1: Convert Word doc pages to images
python src/split_and_image.py

# Step 2: Extract structured JSON from each case image
python src/stage1_claude_vision.py

# Step 3: Map JSON to the 88-column Excel schema
python src/stage2_clinical_reasoning_auditor.py
```

---

## Development Approach

The pipeline went through 13 iterations across two architectural families — docx text parsing (v0–v10) and vision-based extraction (v11–v12). See [`docs/pipeline-evolution.md`](docs/pipeline-evolution.md) for a summary, or browse [`iterations/`](iterations/) to explore the code and approach for any specific version.

---

## Tech Stack

- **Python** — `python-docx`, `pandas`, `openpyxl`, `sentence-transformers`, `medspacy`
- **Anthropic Claude** — claude-3-haiku (vision extraction), claude-3-5-sonnet (reasoning)
- **Validation** — fuzzy matching, exact match, hallucination detection via source provenance tracing

---

*Data: 100% synthetic, anonymised. NHS numbers prefixed NNN. Built as part of the Clinical AI Hackathon, March 2026.*
