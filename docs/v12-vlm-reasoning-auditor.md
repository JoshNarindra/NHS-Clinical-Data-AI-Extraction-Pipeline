# v12 — VLM Reasoning Auditor (Final)

## Approach
Built on v11's vision extraction with a second LLM pass designed to reason clinically rather than map mechanically. Stage 2 operates as a UK NHS colorectal consultant — reading the extracted JSON and determining where each fact belongs in the patient's longitudinal timeline before writing it to the Excel schema.

## Implementation
Stage 1 remains unchanged from v11: Claude vision extracts a JSON per case. Stage 2 sends that JSON to Claude's text API with a clinical reasoning prompt. The model cross-references `clinical_details` and `mdt_outcome` to build a mental map of the patient's journey — distinguishing baseline staging from restaging, routing post-neoadjuvant data to the correct follow-up columns, and identifying treatment decisions. The 88-column schema was split into three clinical chapters (Presentation & Baseline, Treatment, Follow-up) to keep each reasoning call focused. Any value inferred rather than directly stated is written with yellow cell highlighting and an Excel comment explaining the reasoning, providing a transparent audit trail for clinical review.

## Results
Overall accuracy: **76.6%**. Demographics 97.2% at 100% coverage. MDT treatment approach 94.7%. CT and MRI accuracy 100% when extracted — coverage gaps for imaging trace to Stage 1 not structuring those fields separately, a known fix. 78% of all errors originate at Stage 1; Stage 2 reasoning introduced only 22% of failures.

## Remaining gaps
The primary improvement path is a Stage 1 prompt update to extract CT and MRI findings into dedicated JSON fields rather than burying them in unstructured prose. This is expected to significantly improve imaging coverage without changing the Stage 2 architecture.
