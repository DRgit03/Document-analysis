# 🧾 Document Analysis Using Python PDF Libraries

This project demonstrates a backend approach to extract, analyze, and benchmark table/text data from financial documents (like quarterly reports) using three powerful Python libraries:

- 📄 **pdfplumber**
- 📄 **PyMuPDF (fitz)**
- 🧠 **Docling (LLM-based extraction)**

---

## 🔧 Packages Used

| Package      | Purpose                                      |
|--------------|----------------------------------------------|
| `pdfplumber` | Accurate table + text extraction from PDFs   |
| `PyMuPDF`    | PDF parsing, structured block detection      |
| `docling`    | LLM-powered semantic document understanding  |

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
   - Convert financial PDFs to Markdown via LLM
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
