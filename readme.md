# Photo to Clean PDF (`photo-to-clean-pdf`)

An AI agent skill that transforms raw camera photos of physical documents, exam sheets, and worksheets into **clean, flat, shadow-free, print-ready PDF files** — either as a restored document scan or a freshly typeset vector PDF complete with extracted diagrams and logos.

## Key Features

- **Perspective Correction & Page Flattening**: Converts tilted, curled, or angled tabletop photos of paper sheets into flat, top-down digital document pages on a pure white background.
- **Shadow & Handwritten Mark Removal**: Eliminates harsh lighting gradients, phone shadows, paper creases, and handwritten annotations (such as crosses `X`, checkmarks, grading scores, and student notes) while cleanly restoring the underlying printed question numbers and text.
- **Dual PDF Export Modes**:
  1. **Cleaned Scan PDF Mode**: Combines the restored, shadow-free document images sequentially into a multi-page PDF ready for printing.
  2. **Retyped Vector PDF Mode**: Crops and cleans embedded diagrams and school/organization logos, transcribes all printed questions and headers into crisp vector typography, fixes obvious spelling typos from the original print, and compiles a publication-grade PDF.

## Mode Comparison

| Mode | Best Used For | Output Characteristics |
| :--- | :--- | :--- |
| **Cleaned Scan PDF** | Fast cleanup of photographed worksheets or exams | Preserves exact original visual layout on a clean white background |
| **Retyped Vector PDF** | Creating a fresh master copy of an exam or handout | Razor-sharp vector fonts, corrected typos, and extracted high-contrast diagrams |

## Repository Structure

```text
photo-to-clean-pdf/
├── SKILL.md      # Core skill definition and agent workflow (Required)
├── README.md     # Documentation and usage guide
└── LICENSE       # Open-source MIT License
```

## Installation

### Option 1: Install via CLI / Skill Registry

If your environment supports installing skills directly from a GitHub repository:

```bash
npx skills add <your-github-username>/photo-to-clean-pdf
```

### Option 2: Manual Upload via Skills Manager

1. Download `SKILL.md` from this repository (or download the full repository as a `.zip` archive).
2. Open the **Skills Manager** in your AI workspace and upload `SKILL.md` (or the `.zip` file).
3. Alternatively, attach `SKILL.md` directly in a chat session and prompt:
   > *"Please save this SKILL.md file as a new skill."*

## Example Prompts

Attach one or more photos of your document or exam pages and use prompts such as:

- **Clean and export directly to PDF (English)**:
  > *"Clean and straighten these 3 exam page photos into flat white document pages, remove all shadows and handwritten cross marks, and export them as a PDF."*

- **Retype all questions and diagrams into a vector PDF (English)**:
  > *"Retype all the questions from these photos neatly into a new PDF, including the header logo and all biological diagrams."*

- **Clean and export to PDF (Indonesian)**:
  > *"Tolong rapikan 3 foto lembar soal di atas menjadi kertas putih bersih tanpa bayangan, hilangkan semua tanda silang dan coretan, lalu ekspor ke PDF."*

## How It Works

1. **Visual Inspection**: Analyzes uploaded photos to detect page order, numbering sequence, diagrams, and handwritten markings.
2. **Document Restoration**: Generates flat, top-down, pure-white document pages free of shadows and pen/pencil marks, verifying that sequential numbering remains intact.
3. **Asset Extraction & Typesetting**: Crops diagrams using verified coordinate grids, normalizes background pixels to pure white (`#FFFFFF`), and compiles either a direct multi-page scan PDF or a headless Chromium vector PDF.
4. **Quality Verification**: Renders page previews to inspect vertical balance, margin cleanliness, and diagram legibility before delivering the final PDF.

## License

Distributed under the **MIT License**. Free to use, modify, and redistribute.