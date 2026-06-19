# v11 — VLM Direct Logic

## Approach
A fundamental architecture shift. Instead of parsing the Word document as text, each MDT case was rendered as an image and passed to a vision language model. The model sees the form the way a clinician does — as a 2D visual layout — and extracts structured data directly from the image.

## Implementation
Each case table in the Word document was converted to a PNG image. Claude 3 Haiku (vision) processed each image with a fixed JSON schema prompt, returning a structured object per case containing demographics, staging, diagnosis, clinical details, and MDT outcome. A deterministic Stage 2 mapper then translated each JSON into the corresponding Excel row.

## Shortcomings
Vision extraction immediately solved the spatial awareness problem — the model correctly read form layouts that had broken every docx-based approach. However Stage 2 remained a simple deterministic mapper with no clinical reasoning. It had no ability to distinguish a baseline T-stage from a post-treatment T-stage within the same JSON blob. Coverage for imaging fields (CT, MRI) was also low because the Stage 1 prompt captured them in a single unstructured "clinical details" text blob rather than dedicated structured fields, giving Stage 2 no clean data to map from.
