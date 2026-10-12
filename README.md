# Serverless AI Invoice Processing Pipeline

An event-driven, zero-cost data ingestion pipeline built in n8n that automatically captures multi-format invoices (PDF, Image, Text) from email attachments, extracts structured data using AI and OCR, audits financials, and securely logs artifacts to cloud storage and a relational database.

## 🏗 Architecture & Data Flow

The pipeline operates entirely hands-free, triggered by incoming emails, and dynamically routes documents based on MIME types.

`Gmail Trigger` ➔ `JS Payload Bouncer` ➔ `MIME Switch Router` ➔ `AI/OCR Extraction` ➔ `Financial Logic Audit` ➔ `PostgreSQL & Cloudinary`

### Core Components:
* **Ingestion:** Google Workspace API (Gmail Trigger)
* **Validation:** Custom JavaScript (Payload Normalization & MIME validation)
* **Processing Routing:** n8n Switch logic
* **Data Extraction:** Groq OSS LLM (JSON schema extraction) & OCR.space API
* **Storage:** PostgreSQL (Neon) & Cloudinary API

## 🚀 Key Technical Challenges Solved

* **Dynamic Payload Normalization:** Engineered a custom JavaScript "bouncer" script to intercept incoming email payloads, validate MIME types, and normalize deeply nested JSON structures before passing them downstream.
* **Bypassing Multipart Upload Constraints:** When an external OCR API failed to handle standard `multipart/form-data` file uploads, I built a reliable fallback by converting raw binary files to Base64 strings in-memory and injecting them directly into the API request body.
* **Automated Financial Auditing:** Implemented inline business logic to mathematically verify invoice subtotals against tax arrays, flagging math mismatches (e.g., `MATH_MISMATCH`) to prevent dirty data from entering the PostgreSQL database.
* **Cost-Optimized Architecture:** Designed the entire architecture utilizing free-tier API endpoints and intelligent routing—bypassing expensive API calls when native text extraction is available (e.g., for standard PDFs and TXT files).

## 🛠 Setup & Installation

You can easily run this entire pipeline on your local or cloud n8n instance.

**1. Clone the repository:**
```bash
git clone [https://github.com/vincentz89/serverless-ai-invoice-pipeline.git](https://github.com/vincentz89/serverless-ai-invoice-pipeline.git)