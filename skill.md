---
name: photo-to-clean-pdf
description: Convert photos of physical documents, exam sheets, or worksheets into clean, flat, shadow-free PDF files. Use when the user uploads one or more document photos and asks to clean, straighten, remove shadows or handwritten marks (such as crosses, checkmarks, or scores), retype questions and diagrams, or export the result as a PDF.
allowed-tools: image_gen default_api google
---
# Photo to Clean PDF

Transform raw camera photos of physical documents, exam papers, or worksheets into clean, print-ready PDF files by removing perspective tilt, shadows, and handwritten markings, then compiling the pages into either a cleaned scan PDF or a freshly typeset vector PDF.

## When to Use

- The user uploads one or more photos of paper documents, worksheets, or exam sheets and asks to convert or export them to PDF.
- The user wants camera shadows, perspective distortion, paper creases, or handwritten markings (crosses, checkmarks, scores, or notes) removed from document photos before exporting to PDF.
- The user asks to retype or rewrite the text and diagrams from photographed document pages into a neat, print-ready PDF.

## Steps

### 1. Inspect Uploaded Photos

Examine the uploaded images to identify the page order, document structure (headers, logos, numbered questions, multi-column layouts, diagrams), and any visual defects that need cleaning (shadows, skew, handwritten crosses, checkmarks, or grading notes).

### 2. Clean and Straighten Document Photos

For each uploaded photo, call `image_gen:generate_image` with the uploaded image in `image_references` and `aspect_ratio` set to `"3:4"` (for portrait document pages):

- Instruct the image generator to transform the photo into a flat, top-down digital document scan on a pure white background with no surrounding table or borders.
- Explicitly specify removing all shadows, paper creases, and handwritten markings (crosses, checkmarks, handwritten names, or scores) while restoring the underlying printed question numbers and option letters cleanly.
- Preserve all original printed text, headers, logos, and diagrams in their proper sequential order.
- Inspect the generated images to verify that sequential numbering and printed text were preserved accurately. If numbering shifted, run a targeted follow-up edit on that page.

### 3. Build the PDF Deliverable

Look up the `pdf-processing` and `file-generation` skills first. Choose the output mode based on the user's request (or provide the cleaned image PDF by default and offer the retyped vector PDF when requested):

- **Mode A - Direct Cleaned Scan PDF (Default for fast photo-to-PDF conversion)**:
  - Write a Python script in `artifacts/file_generation/ttl=63d/intermediate/` using `Pillow` (`PIL.Image`) to load all cleaned page images in sequential order, convert them to `"RGB"`, and save them into a single multi-page PDF in `artifacts/file_generation/ttl=63d/output/` (`save_all=True`, `append_images=...`).

- **Mode B - Retyped Vector PDF with Extracted Diagrams (When asked to retype or rewrite neatly)**:
  - Extract any logos and diagrams from the cleaned white-background images using `Pillow` crop boxes. Normalize near-white pixels to pure white (`#FFFFFF`) so the cropped illustrations blend seamlessly into the page.
  - Generate a contact sheet or coordinate grid in `artifacts/file_generation/ttl=63d/intermediate/` and inspect it with `default_api:view_file` to verify that every cropped diagram is complete and free of stray text.
  - Transcribe all document headers, instructions, and numbered items into a clean HTML/CSS document (`artifacts/file_generation/ttl=63d/intermediate/document.html`), fixing obvious spelling typos from the original print while keeping all content faithful.
  - Arrange diagrams and question stems in a balanced layout (such as a compact borderless side-by-side table for items with illustrations) so pages fill evenly without large bottom gaps.
  - Compile the HTML to a vector PDF in `artifacts/file_generation/ttl=63d/output/` using `compile_and_render.py`.

### 4. Verify and Present the PDF

- Render preview PNGs of the generated PDF pages into `artifacts/file_generation/ttl=63d/intermediate/` and inspect them in parallel with `default_api:view_file` to confirm page order, legibility, diagram alignment, and clean margins.
- Present the finished PDF from `artifacts/file_generation/ttl=63d/output/` using `google:show_file` and include the resulting file tag in the response.

## Gotchas

- **Sequential numbering drift during image cleanup**: When handwritten crosses cover printed question numbers, image generation may misnumber items in a column. Always verify the numbering sequence across columns and run a quick edit if any numbers were skipped.
- **Blind diagram cropping**: Never guess crop coordinates for embedded diagrams. Always overlay a coordinate grid first and verify the cropped assets on a contact sheet via `default_api:view_file` before compiling the final PDF.
- **Page break gaps on diagram-heavy exams**: Stacking large diagrams vertically above long multiple-choice options inside an unbreakable row can cause premature page breaks. Place diagrams side-by-side with the question stem and options in a borderless table to keep vertical height compact.
