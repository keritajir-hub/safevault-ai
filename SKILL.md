---
name: safevault-ai
description: Zero-cloud local document organizer, PII redaction tool, and emergency metadata parser for UGREEN AI NAS.
---

# SafeVault AI — Local Document & Medical Vault

## Overview
SafeVault AI is an automated local file manager that processes sensitive family and personal documents stored on UGREEN AI NAS without sending any data to external cloud services.

## Required Inputs
- Target Folder Path: `/volume1/SafeVault_Drop/`
- Output Folder Path: `/volume1/SafeVault_Storage/`

## Supported UGOS CLI Capabilities
- `ugos-cli file list --path <path>`
- `ugos-cli file move --src <src> --dest <dest>`
- `ugos-cli file mkdir --path <path>`
- `ugos-cli metadata set --file <path> --tags <tags>`

## Expert Agent Orchestration Workflow
1. **File Watcher & Ingestion:**
   - Scan `/SafeVault_Drop` for new image (`.png`, `.jpg`, `.jpeg`) or PDF files.
2. **Local Vision & OCR Analysis:**
   - Run local OCR engine to extract document text.
   - Detect document category (Medical, Insurance, ID, Tax, Receipt).
3. **PII Masking & Dual-Copy Generation:**
   - Identify sensitive fields (national ID numbers, personal signatures, sensitive medical diagnosis).
   - Create a redacted file copy in `/SafeVault_Storage/Shared_Redacted/` with blacked-out PII bounding boxes.
4. **Filing & CLI Execution:**
   - Move original file to `/SafeVault_Storage/<Category>/<YYYY>_<DocName>.<ext>` using `ugos-cli file move`.
   - Apply tags using `ugos-cli metadata set`.
5. **Emergency Metadata Update:**
   - Append extracted high-level summary (e.g., Policy Number, Renewal Date) to local `emergency_summary.json`.

## Completion and Failure Handling
- **Success:** Log file path and metadata entry to local log file `safevault.log`.
- **Failure (Unreadable Document):** Move unprocessable files to `/SafeVault_Storage/Manual_Review/` and log failure reason.

## Third-Party Disclosures & AI Tools
- **Tesseract OCR / OpenCV:** Open-source image processing libraries (Apache 2.0 / BSD License).
- **AI Tool Usage:** Agent orchestration workflow designed with AI Assistant assistance for standard UGOS Skill specification compliance.
