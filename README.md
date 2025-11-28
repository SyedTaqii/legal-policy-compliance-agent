# ⚖️ Legal Policy Compliance Agent

AI Audit & Compliance System

## Overview
This repository implements an AI-powered contract auditor that checks contracts against a set of 15 legal compliance rules (e.g., Indemnification, Governing Law, Force Majeure). It retrieves clause-level evidence from a FAISS vectorstore and evaluates each rule using an LLM to produce a structured CSV audit report with Pass/Fail determinations and supporting evidence.

## Key Features
- Automated auditing across 15 customizable legal rules
- Evidence retrieval via a FAISS vectorstore (`policy_vectorstore/`)
- Logic-based verdicts: `COMPLIANT` or `NON-COMPLIANT`
- CSV report containing verdicts and cited evidence

## Tech Stack
- LLM: Google Gemini Pro (gemini-pro)
- Orchestration: LangChain
- Vector DB: FAISS
- Dataset: CUAD v1 (Contract Understanding Atticus Dataset)
- Notebook-based workflow for ingestion & auditing

## Prerequisites
- Python 3.10+
- Git
- (Optional) Jupyter / JupyterLab

## Installation (Windows)
1. Clone the repository:
   - PowerShell / CMD:
     git clone https://github.com/SyedTaqii/legal-policy-compliance-agent.git
     cd legal-policy-compliance-agent

2. Create and activate a virtual environment (recommended):
   - PowerShell:
     python -m venv .venv
     .\.venv\Scripts\Activate.ps1
   - CMD:
     python -m venv .venv
     .\.venv\Scripts\activate.bat

3. Install dependencies:
   pip install -r requirements.txt

## Configuration
1. Create a `.env` file in the project root with your API key(s):
GOOGLE_API_KEY=your_actual_api_key_here
Adjust variable names to match the LLM provider setup in your code.

2. Prepare CUAD dataset:
- Place the CUAD_v1.json (or other contract JSONs) in `CUAD_v1/` or update the notebook/script paths accordingly.

## Usage
This project is driven from a Jupyter Notebook (`genai4task2.ipynb`) for ingestion and auditing.

1. Ingest contracts (Notebook cell 1):
- Reads contract text(s)
- Chunks text and builds/saves FAISS vectorstore to `policy_vectorstore/`

2. Run the audit (Notebook cell 2):
- Iterates the 15 rules from `compliance_rules.json`
- Performs retrieval + LLM evaluation for each rule
- Writes results to `compliance_report.csv`

Open `compliance_report.csv` to review verdicts and evidence text.

## File Layout
- genai4task2.ipynb       — Main notebook (ingestion + audit)
- compliance_rules.json   — Definitions for the 15 rules
- compliance_report.csv   — Generated audit results
- policy_vectorstore/     — FAISS index directory (created by ingestion)
- requirements.txt        — Python dependencies
- README.md               — This document

## Notes & Recommendations
- The full CUAD dataset is large; the repo does not include it. Download CUAD v1 from the Atticus Project and place the file under `CUAD_v1/`.
- Adjust chunk size, overlap, and FAISS index settings in the notebook to match your document sizes and memory constraints.
- Validate and secure API keys; avoid committing `.env` to version control.

## Troubleshooting
- FAISS import errors: ensure the correct binary wheel for your Python version/OS (Windows builds may require conda or prebuilt wheels).
- LLM connectivity: verify `GOOGLE_API_KEY` and network access.
- If results are unexpected, increase retrieval top_k or improve chunking to preserve clause context.
