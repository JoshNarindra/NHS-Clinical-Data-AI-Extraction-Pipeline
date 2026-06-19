# Clinical AI Extraction Pipeline

An AI pipeline that extracts structured clinical data from NHS Multidisciplinary Team (MDT) meeting documents and populates a longitudinal patient database — turning unstructured Word documents into a searchable, auditable Excel database.

---

## The Problem

NHS oncology MDT meetings produce semi-structured Word documents containing patient histories, staging, imaging results, and treatment decisions. This data is effectively locked — difficult to query, audit, or use for research. Clinicians resort to maintaining manual databases, which is time-consuming and inconsistent.

**The task:** given 50 synthetic MDT proformas, automatically extract and populate an 88-column longitudinal patient database matching a clinician-defined schema.

*Problem statement by Dr Anita Wale, Consultant Radiologist, St George's University Hospitals NHS Foundation Trust.*

---

## Pipeline Architecture

The final pipeline (v12) uses two stages:

```
Word Document (50 MDT cases)
        │
        ▼
  [Stage 1: Claude Vision]
  Each case rendered as an image and sent to a vision LLM.
  Extracts demographics, staging, imaging, and MDT outcome into structured JSON.
        │
        ▼
  JSON (one file per patient)
        │
        ▼
  [Stage 2: Clinical Reasoning Auditor]
  An LLM acting as a UK NHS colorectal consultant.
  Cross-references the patient timeline to route facts to the correct columns
  (e.g. distinguishes baseline MRI from post-treatment restaging MRI).
  Flags any inferred values with cell comments for human review.
        │
        ▼
  Final Excel Database (88 columns, 50 patient rows)
```

---

## Results

Validated against the source Word document across all 50 cases.

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

Error attribution: 78% of all errors originate at Stage 1 (vision extraction), 22% at Stage 2 (mapping). The pipeline is accurate when it extracts data; the primary improvement path is increasing Stage 1 coverage for imaging fields.

---

## Repository Structure

```
├── src/                    # Final pipeline (v12)
│   ├── split_and_image.py           # Converts Word doc pages to images
│   ├── stage1_claude_vision.py      # Vision extraction → JSON
│   ├── stage2_clinical_reasoning_auditor.py  # LLM reasoning → Excel
│   ├── stage2_hybrid_clinical_auditor.py     # Hybrid variant
│   ├── fix_json_logic.py            # JSON repair utilities
│   └── config.py                    # API keys and paths
├── data/
│   ├── hackathon-mdt-outcome-proformas.docx  # Input: 50 synthetic MDT cases
│   └── hackathon-database-prototype.xlsx     # Ground truth schema
├── output/
│   └── final_clinical_database.xlsx          # Pipeline output
├── validation/
│   ├── validate_final.py            # Validation script
│   ├── gold_standard_template.json  # Annotated ground truth for case 0
│   └── Final Validation             # Full results and methodology
├── docs/
│   ├── Problem_Statement.md         # Clinical problem brief
│   ├── specification.md             # Technical specification
│   └── *.png                        # Input/output format examples
└── iterations/
    ├── README.md                    # Summary of all 13 approaches tried
    └── v0/ through v12/             # Full code and outputs per iteration
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

The pipeline went through 13 iterations across two architectural families:

- **v0–v10:** Python-docx based extraction using progressively more sophisticated NLP — regex, MedSPaCy NER, sentence embeddings, and longitudinal patient linking. Best result: 1,102 cells at high recall.
- **v11–v12:** Vision-based architecture. Rendered document pages as images and used a multimodal LLM (Claude) for extraction, bypassing all docx parsing complexity. Added a clinical reasoning layer in v12 to handle timeline disambiguation.

See [`iterations/README.md`](iterations/README.md) for a full breakdown of each approach and its results.

---

## Tech Stack

- **Python** — `python-docx`, `pandas`, `openpyxl`, `sentence-transformers`, `medspacy`
- **Anthropic Claude** — claude-3-haiku (vision extraction), claude-3-5-sonnet (reasoning)
- **Validation** — fuzzy matching, exact match, hallucination detection via source provenance tracing

---

*Data: 100% synthetic, anonymised. NHS numbers prefixed NNN. Built as part of the Clinical AI Hackathon, March 2026.*
