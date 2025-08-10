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


Notes & Limitations
RAG depends on the uploaded ADGM reference docs: better/more relevant references = better AI citations.

If RAG/Gemini returns non-JSON or fails, the app falls back to rule-based checks and still returns outputs.

Inline review notes are inserted as visible red inline tags (easily visible in Word). If you want Word comment balloons (native comment objects), that requires additional XML edits; ask and it can be added.




## Run the App
