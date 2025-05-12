# 🧾 Document Analysis Using Python PDF Libraries

This project demonstrates a backend approach to extract, analyze, and benchmark table/text data from financial documents (like quarterly reports) using three powerful Python libraries:

- 📄 **pdfplumber**
- 📄 **PyMuPDF (fitz)**
- 🧠 **Docling (traditional structured parsers and fine-tuned models from Hugging Face if we are working in advanced setting with metadata)**

---

## 🔧 Packages Used

| Package      | Purpose                                      |
|--------------|----------------------------------------------|
| `pdfplumber` | Accurate table + text extraction from PDFs   |
| `PyMuPDF`    | PDF parsing, structured block detection      |
| `docling`    | traditional structured parsers  |

---

## 📁 Notebook Overview

The `3packages_backend_understanding.ipynb` file contains:

1. **Text & Table Extraction (pdfplumber):**
   - Loop through each page
   - Extract page-wise text
   - Extract and display tabular data
   - Store performance timing

2. **Text Block & Table Analysis (PyMuPDF):**
   - Analyze font sizes to find section titles
   - Extract and label each table
   - Measure performance time

3. **Semantic Parsing (Docling):**
   - Convert financial PDFs to Markdown via traditional structured parsers.
   - Use regex to extract table from the markdown text
   - Time the steps: initialization, conversion, export

---

## 📊 Profiling Metrics

Each method includes:
- Execution time benchmarking (`time.time()`)
- Print/display of extracted tables
- Optional conversion to CSV or JSON (extensible)

---

## 📂 Directory Structure

```
Documents_analysis_with_multiple_packages/
│
├── 3packages_backend_understanding.ipynb  # Main notebook
├── tables/                                # (Optional) extracted tables CSV
├── README.md                              # Project summary
└── requirements.txt                       # Dependencies
```

---

## 📌 How to Run

1. Install dependencies:

```bash
pip install pdfplumber PyMuPDF docling pandas
```

2. Run the notebook:

```bash
jupyter notebook 3packages_backend_understanding.ipynb
```

3. (Optional) Extract tables to `.csv` and visualize performance metrics.

---

## 📈 Use Case

Designed for analysts or engineers needing:
- PDF and scanned Pdf financial document parsing
- Automated extraction of Income Statements, Balance Sheets
- Verification of values (e.g., matching Net Income)
- Comparison across quarterly financial results

---
# IBM Docling: OCR-Enabled PDF Parsing Guide

## Introduction

IBM Docling is a **free and open-source** document parsing tool that excels in extracting structured content from documents—such as text, tables, and images—without using large language models (LLMs) during the parsing phase. This unique design provides several benefits in terms of **cost, privacy**, and **control**.

---

## Key Features

### 1. Free and Open Source

Docling is completely free to use and can be installed locally. This makes it accessible to both individuals and organizations without any commercial license or subscription fees.

### 2. No Use of LLMs During Parsing

Unlike tools such as LlamaParse that depend on LLMs to parse documents, Docling avoids using any large models during parsing:

* **No token cost:** Using LLMs incurs token usage and cost, which Docling avoids.
* **Privacy-safe:** Tools using LLMs often send your data to cloud-based models, creating potential privacy concerns. Docling ensures your sensitive documents remain local.
* **Security:** Whether LLMs use private or public infrastructure, they can potentially see and store document contents. Docling sidesteps this entirely by working locally.

### 3. Structured Extraction Powered by Hugging Face Models

Docling uses specialized, fine-tuned extractors from Hugging Face to detect and structure tables, text blocks, figures, and other visual elements:

* Provides output in structured formats.
* Enables precise alignment of text with tables and images.
* Great for downstream tasks like feeding into language models, generating analytics, or document summarization.

---

## Use Case Benefits

* Docling outperforms other open-source alternatives like PyMuPDF and Unstructured when it comes to extracting and organizing **complex document structures**.
* Particularly effective for **financial presentations**, **scanned documents**, **invoices**, **balance sheets**, and **multi-format files**.

---

## Advanced Feature Support

To achieve advanced extraction with Docling, you need to configure pipeline options:

* **Images Scale & Generation:**

  * Convert pages into high-resolution images.
  * Enables use with vision-enabled language models.

* **Pipeline Customization:**

  * Use `PdfPipelineOptions` to activate OCR, table extraction, image capture, and other options.
  * Extracted pages and text units carry metadata such as `page_no`, `reference id`, and type (text, image, table).

* **Document Type Awareness:**

  * Docling supports parsing of **PDF**, **PPTX**, **DOCX**, and **HTML** formats.
  * You can access the document’s `mimetype`, `filename`, and more metadata.

---

## OCR Pipeline Explanation

Some documents—especially scanned ones—contain text that cannot be extracted using standard parsers. OCR becomes essential in these cases. Below is a breakdown of how Docling enables OCR:

### Pipeline Configuration Components

* `do_ocr = True`: Enables Optical Character Recognition.
* `do_table_structure = True`: Enables detection of tables and structural relationships.
* `do_cell_matching = True`: Enables accurate mapping of rows/columns even for misaligned or merged cells.
* `generate_page_images = True`: Generates full-page thumbnails of each document page.
* `ocr_options = EasyOcrOptions(force_full_page_ocr=True)`: Ensures the entire page is processed for OCR, not just regions where text is missing.

### Behavior of `force_full_page_ocr`

| Setting           | Description                                                      |
| ----------------- | ---------------------------------------------------------------- |
| `False` (default) | OCR runs only on scanned/image content.                          |
| `True`            | OCR runs on the entire page regardless of embedded digital text. |

---

## What Does “Text Cannot Be Extracted Normally” Mean?

* **Digital PDF**: Selectable text (e.g., Ctrl+C works) → Docling uses built-in extractors.
* **Scanned PDF**: Pages are images (e.g., photos of printed text) → Requires OCR.
* **Mixed Content**: Partly text, partly scanned → OCR is selectively applied unless forced fully.

---

## Reference Mapping and Metadata

Every document processed through Docling is internally represented as a structured JSON-like model that contains metadata at multiple levels—document, page, and content element. This metadata is critical for automation, indexing, linking, and advanced analysis.

### Core Metadata Elements

Each content block extracted from the document is associated with the following key metadata fields:

* **`self_ref`**: A unique identifier for the content unit (text, image, table, etc.). Useful for referencing or tracking specific items.
* **`page_no`**: Indicates which page the content appears on. Important for reconstructing the document's visual structure.
* **`text`**: The actual content of the text block. This could be a sentence, paragraph, or a fragment, depending on the layout.
* **`type`**: Defines the nature of the content element (e.g., `text`, `table`, `picture`, `title`, `header`, `footer`).
* **`prov`**: A provenance field showing where and how the content was derived (including page number, region, and engine used).
* **`group`**: Used for grouping related content units, such as multi-column text blocks or table-cell clusters.
* **`image`**: For picture or page-based elements, this holds the image data or reference to the image object.

### Examples

#### Example 1: Text Metadata

```json
{
  "self_ref": "text_3ac4",
  "page_no": 2,
  "text": "Revenue increased by 10% year over year...",
  "type": "text",
  "prov": [{"page_no": 2, "source": "ocr"}]
}
```

#### Example 2: Image Metadata

```json
{
  "self_ref": "img_9f1b",
  "page_no": 4,
  "type": "picture",
  "image": {"format": "jpeg", "width": 1200, "height": 1600},
  "prov": [{"page_no": 4}]
}
```

#### Example 3: Table Metadata

```json
{
  "self_ref": "table_b12f",
  "page_no": 3,
  "type": "table",
  "rows": 5,
  "cols": 4,
  "prov": [{"page_no": 3, "source": "structure_extractor"}]
}
```

### Why This Metadata Matters

* **Automated Linking**: Easily map text to associated tables or visual references.
* **Intelligent Chunking**: Group and structure content for LLMs or API pipelines.
* **Search and Indexing**: Build fast and accurate document search systems.
* **Data Provenance**: Track how and where each piece of content was extracted.
* **Audit and Debug**: Inspect pipeline decisions for data validation or correction.

This rich metadata structure makes Docling not just a parser but a foundational tool for robust document understanding and automation workflows.
Every extracted text unit contains:

* `self_ref`: Unique ID to track or map content.
* `page_no`: Which page the content belongs to.
* `text`: The raw content itself.

This enables powerful document analysis features such as:

* **Linking text to images/tables**
* **Chunking for downstream NLP tasks**
* **Precise display and review of document structure**

---

## Visual Image Display

Docling enables rendering of images and scanned pages:

* Page images are helpful when passing data to models like GPT-4V.
* Thumbnails of full pages or pictures in the document can be shown with `display_images()`.

---

## Real-World Applications

* **Financial statement processing**
* **Scanned bill parsing**
* **Earnings report summarization**
* **Regulatory or legal document analysis**

OCR can also resolve errors such as broken characters, misplaced word breaks, or faded text by applying machine-learned techniques like EasyOCR to the full page.

---

## Summary Table of OCR Configuration Options

| Option                        | Description                                                      |
| ----------------------------- | ---------------------------------------------------------------- |
| `do_ocr`                      | Enables OCR on scanned or image-based pages                      |
| `do_table_structure`          | Turns on advanced table boundary and layout detection            |
| `do_cell_matching`            | Matches complex tables accurately with cell coordinates          |
| `force_full_page_ocr = False` | OCR only on missing areas (default)                              |
| `force_full_page_ocr = True`  | Full-page OCR on all content for maximum capture                 |
| `generate_page_images`        | Outputs images of each document page for visualization/debugging |

---

## Additional Insight on OCR Performance

In scenarios involving scanned or partially corrupted documents, Docling's OCR capability shines. If there is any misinterpreted or broken text—such as hyphenated or separated words—feeding the OCR-parsed output to a language model (LLM) can significantly improve the outcome. The LLM can intelligently:

* Fill in missing parts
* Merge incorrectly split text spans
* Correct layout inconsistencies

Even though the raw OCR output might have minor issues, the overall content extracted is accurate and usable for further processing.

## Conclusion

Docling provides a powerful and privacy-safe alternative to LLM-powered document parsing tools. It is ideally suited for users dealing with sensitive documents and needing high-accuracy parsing, structured extraction, and integration with downstream AI models.

By leveraging Hugging Face-based extractors and local OCR engines like EasyOCR or Tesseract, Docling offers a strong balance of accuracy, flexibility, and control.

If you're processing documents at scale or working in finance, law, or research, Docling’s structured parsing pipelines are a perfect fit.

