# v11 — VLM Direct Logic

## Approach
A fundamental architecture shift. Each MDT case rendered as an image and passed to a vision language model. The model sees the form as a clinician does — as a 2D visual layout — and extracts structured data directly from the image, bypassing all docx parsing.

## Implementation
Each case table in the Word document converted to a PNG. Claude 3 Haiku (vision) processed each image with a fixed JSON schema prompt, returning a structured object per case containing demographics, staging, diagnosis, clinical details, and MDT outcome. A deterministic Stage 2 mapper translated each JSON into the corresponding Excel row.

## Shortcomings
Vision extraction immediately resolved the spatial awareness problem. However Stage 2 was a simple deterministic mapper with no clinical reasoning — it couldn't distinguish a baseline T-stage from a post-treatment one within the same JSON. Coverage for imaging fields was also low because the Stage 1 prompt captured CT and MRI data in a single unstructured text blob rather than dedicated fields, leaving Stage 2 nothing structured to map from.
