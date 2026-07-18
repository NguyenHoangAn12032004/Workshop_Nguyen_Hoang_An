---
title: "Blog 2: AI-Powered Content Pipeline"
date: 2026-07-07
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# From DOCX to Exam Questions — Building an AI-Powered Content Pipeline with Lambda, S3, and Tesseract

<!-- Replace with your actual blog post link -->
**Published on:** [AWS Study Group](https://cloudjourney.awsstudygroup.com/)

## Summary

This blog post details how LingoRise's admin content pipeline automates the transformation of DOCX, PDF, and scanned-PDF files into structured exam questions — reducing content creation time from hours to minutes.

## Key Topics Covered

### The Problem
Exam content creators work with DOCX and PDF files. Manually reformatting these into structured database records is tedious, error-prone, and doesn't scale.

### The Pipeline Architecture
```
DOCX/PDF Upload → S3 → Lambda (Parser) → Structured JSON
                                ↓
Scanned PDF → S3 → OCR Worker (Tesseract.js) → Text → Parser → JSON
                                ↓
Structured JSON → PostgreSQL (question_bank table)
                                ↓
Admin Review → Duplicate Detection → Publish
```

### Implementation Details

#### Document Parsing
- **DOCX files**: Parsed using `mammoth` library to extract structured HTML, then converted to exam question format.
- **PDF files**: Parsed using `pdf-parse` for text extraction.
- **Scanned PDFs**: Queued for OCR processing via Tesseract.js worker.

#### OCR Worker Queue
The OCR system uses a background worker pattern:
- Documents needing OCR are flagged with `ocr_status: 'queued'` in the database.
- A worker (`npm run worker:ocr`) processes the queue one document at a time.
- Watch mode (`npm run worker:ocr:watch`) continuously polls for new documents.
- Configurable timeout (default 60s) and language settings.

#### Duplicate Detection
Before publishing, the system checks for duplicate questions using text similarity to prevent redundant content in the question bank.

#### Audit Logging
Every content change is tracked: who imported what, when, and which questions were created, modified, or rejected.

### AWS Services Involved
| Service | Role in Pipeline |
|---------|-----------------|
| **S3** | Stores uploaded documents and processed assets |
| **Lambda** | Runs the parsing and import logic |
| **CloudWatch** | Monitors OCR worker health and processing times |
| **SES** | Notifies admins when imports complete or fail |

### Results
- Content import time reduced from ~2 hours (manual) to ~5 minutes (automated).
- OCR accuracy sufficient for most printed exam materials.
- Admin review step ensures quality control before publishing.