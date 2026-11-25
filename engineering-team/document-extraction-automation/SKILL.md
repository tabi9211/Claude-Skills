---
name: document-extraction-automation
description: Turn PDFs, scanned images, and Office documents into structured Markdown, JSON, or field-level data (invoices, forms, reports) using Docling and docext, with confidence scoring for downstream automation like AP processing. Use when a user wants to extract text/tables from documents, build a document extraction pipeline, benchmark OCR tools, or automate invoice/field capture.
version: 1.0.0
author: borghei
category: engineering-team
tags: [ocr, document-extraction, docling, invoices, automation]
---

# Document Extraction Automation

Two complementary open-source tools for turning unstructured documents into structured data, plus guidance on when each fits and how to benchmark them against a vendor option (e.g. a BPO's OCR offering) before committing to one.

---

## The Two Tools

| Tool | Best for | Output | Confidence scores |
|---|---|---|---|
| **Docling** (`docling-project/docling`, IBM) | General document conversion — PDFs, DOCX, PPTX, images, HTML — into clean Markdown/JSON for RAG pipelines, search, or further LLM processing | Markdown, JSON, HTML, with layout and table structure preserved | No — it's a converter, not a field extractor |
| **docext** (`NanoNets/docext`) | Pulling specific *fields* and *tables* out of structured documents (invoices, receipts, forms) without a separate OCR step | Structured key-value fields + tables | Yes — per-field confidence, useful for flagging low-confidence extractions for human review |

**Rule of thumb:** use Docling when the goal is "get this document into a form an LLM or search index can use." Use docext when the goal is "pull invoice number, vendor, total, line items out of this specific document type."

---

## Installing

Both are regular Python packages — no GPU required for baseline use, though larger models benefit from one.

```bash
# Docling
pip install docling

# docext
pip install docext
```

Note: Docling's default pipeline pulls in several ML dependencies (layout + table models via Hugging Face); first run downloads model weights, so expect the first conversion to be slower than subsequent ones. Check available disk space before installing on a constrained environment.

---

## Docling: Quick Usage

```python
from docling.document_converter import DocumentConverter

converter = DocumentConverter()
result = converter.convert("invoice.pdf")
print(result.document.export_to_markdown())
```

CLI equivalent:
```bash
docling invoice.pdf --to md --output ./out
```

What it preserves: reading order, headings, tables (as proper Markdown/HTML tables, not flattened text), and figure/caption association. This matters for downstream chunking — a converter that flattens tables into plain text silently destroys the structure an LLM needs to answer "what was line 3's amount."

---

## docext: Quick Usage

```bash
python -m docext.app.app  # launches a local UI for field-extraction templates
```

Or programmatically, define the fields you expect (vendor, invoice_number, total, line_items) and docext returns each with a value and a confidence score. Route anything below your chosen confidence threshold to manual review rather than trusting it silently — that threshold is a business decision (balance of review cost vs. error cost), not a fixed default.

---

## Benchmarking Against a Vendor Option

If evaluating these against a paid tool (e.g. a BPO's packaged OCR/extraction option) for an AP automation decision:

1. Pull a representative sample of real documents — mixed quality, mixed formats, including the messy ones, not just clean PDFs.
2. Run the same sample through Docling/docext and the vendor tool.
3. Score each on: field-level accuracy, table-structure fidelity, processing time, and cost per document at expected volume.
4. Weight the comparison by what actually breaks the process downstream — a wrong invoice total is far more costly than a slightly mis-ordered line item, so don't treat all errors as equal.
5. Document the false-negative rate (fields it should have found but missed) separately from false-positive (wrong value with high confidence) — they call for different review strategies.

---

## Common Pitfalls

- **Treating Docling's output as field-extracted data.** It gives you clean structure, not named fields — don't skip a field-extraction step (docext, or a prompt against the Markdown) if that's actually what's needed.
- **Trusting high-confidence scores blindly.** docext's confidence reflects model certainty, not ground truth — spot-check a sample even on "high confidence" extractions before fully automating a workflow around them.
- **Skipping the disk/dependency check.** Docling's model downloads can be a few hundred MB to a few GB depending on the pipeline configured — confirm there's headroom before installing in a constrained container or CI environment.
