# ADGM Corporate Agent — README

## Overview

This Streamlit app is an AI-powered **Corporate Agent** for the Abu Dhabi Global Market (ADGM).  
It reviews uploaded `.docx` legal documents, verifies checklists for processes (e.g. Company Incorporation),  
flags legal red-flags, inserts inline review notes into the file, and produces a structured JSON report.

## Features

- **Upload & Parse** — accepts `.docx` documents for review.
- **Process Detection** — recognizes processes such as Company Incorporation using keywords.
- **Checklist Verification** — compares uploaded docs with ADGM checklists and lists missing docs.
- **Red-Flag Detection** — flags jurisdiction issues, missing signature blocks, ambiguous language, etc.
- **Inline Review Notes** — inserts visible review notes (red inline tags) into the document.
- **RAG Integration** — optionally uses uploaded ADGM reference documents and Gemini (Gemini 2.5 Flash) to augment findings and provide citations (falls back to rule-based checks if RAG fails).
- **Outputs** — downloadable reviewed `.docx` and a structured JSON report.

## Requirements

- Python 3.9+
- Virtual environment recommended
- Python packages: `streamlit`, `python-docx`, `google-genai`, `numpy`

## Installation

```bash
# Create and activate a virtual environment
python -m venv venv

# mac / linux
source venv/bin/activate

# windows (PowerShell)
venv\Scripts\Activate

# Upgrade pip and install dependencies
pip install --upgrade pip
pip install streamlit python-docx google-genai numpy
Environment Variables
Set your Gemini API key in the same shell you run Streamlit from.

bash
Copy
Edit
# mac / linux
export GEMINI_API_KEY="your_key_here"

# Windows PowerShell
$env:GEMINI_API_KEY="your_key_here"
Security note: Do not commit your API key to source control. Use environment variables only.

Run the App
bash
Copy
Edit
streamlit run adgm_agent.py
The app will open in your default browser at http://localhost:8501 (or the port Streamlit allocates).

Usage
Optionally upload ADGM reference documents (.docx or .txt) and click Build/Refresh RAG index to enable RAG & citations.

Upload the user .docx files you want reviewed.

If you uploaded references, build the index (embeddings will be created).

The agent runs rule-based checks and (if available) RAG + Gemini to augment results.

Download reviewed .docx files and the JSON report.

JSON Output Example
json
Copy
Edit
{
  "process": "Company Incorporation",
  "documents_uploaded": 1,
  "required_documents": 5,
  "missing_documents": [
    "Register of Members and Directors",
    "UBO Declaration Form",
    "Incorporation Application Form",
    "Memorandum of Association"
  ],
  "issues_found": [
    {
      "section": "language",
      "issue": "Ambiguous use of 'may' detected; could be non-binding",
      "severity": "Low",
      "suggestion": "Consider replacing 'may' with clearer mandatory language if intended.",
      "citation": null,
      "document": "Example.docx"
    }
  ],
  "timestamp": "2025-08-09T18:31:18.823714Z"
}
Submission Checklist
adgm_agent.py — main Streamlit app

README.html (this file)

One example .docx before review and one after review

Generated JSON report

Optional: screenshot or short demo video

Notes & Limitations
RAG depends on the uploaded ADGM reference docs: better/more relevant references = better AI citations.

If RAG/Gemini returns non-JSON or fails, the app falls back to rule-based checks and still returns outputs.

Inline review notes are inserted as visible red inline tags (easily visible in Word). If you want Word comment balloons (native comment objects), that requires additional XML edits; ask and it can be added.
