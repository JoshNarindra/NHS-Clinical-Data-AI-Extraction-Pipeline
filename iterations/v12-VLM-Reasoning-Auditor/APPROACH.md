# v12 — VLM Reasoning Auditor (Final)

## Approach
Built on v11's vision extraction with a second LLM pass that reasons clinically rather than maps mechanically. Stage 2 operates as a UK NHS colorectal consultant — reading the extracted JSON and determining where each fact belongs in the patient's longitudinal timeline before writing it to the Excel schema.

## Implementation
Stage 1 unchanged from v11: Claude vision extracts a JSON per case. Stage 2 sends that JSON to Claude's text API with a clinical reasoning prompt. The model cross-references `clinical_details` and `mdt_outcome` to build a mental map of the patient's journey — distinguishing baseline staging from restaging, routing post-neoadjuvant data to the correct follow-up columns, and identifying treatment decisions. The 88-column schema split into three clinical chapters (Presentation & Baseline, Treatment, Follow-up) to keep each reasoning call focused. Any inferred value written with yellow cell highlighting and an Excel comment explaining the reasoning, providing a transparent audit trail for clinical review.

## Results
Overall accuracy **76.6%**. Demographics 97.2% at 100% coverage. MDT treatment approach 94.7%. CT and MRI accuracy 100% when extracted. 78% of all errors trace to Stage 1; Stage 2 reasoning introduced only 22% of failures.

## Remaining gaps
Primary improvement path: update the Stage 1 prompt to extract CT and MRI findings into dedicated JSON fields rather than unstructured prose. Expected to significantly improve imaging coverage without changing the Stage 2 architecture.
