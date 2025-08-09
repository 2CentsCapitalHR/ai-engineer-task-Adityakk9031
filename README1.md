<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>ADGM Corporate Agent — README</title>
  <style>
    body { font-family: system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial; line-height: 1.6; padding: 24px; color: #111; background:#f8f9fb; }
    main { max-width: 900px; margin: 0 auto; background: #fff; padding: 28px; border-radius: 10px; box-shadow: 0 6px 24px rgba(0,0,0,0.06); }
    h1 { margin-top: 0; color: #0b4a6f; }
    pre { background:#0f1724; color:#dbeafe; padding:12px; border-radius:6px; overflow:auto; }
    code { font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, "Roboto Mono", "Segoe UI Mono", monospace; }
    .section { margin-bottom: 20px; }
    ul { margin-top: 0; }
    .note { background:#fff7cc; border-left:4px solid #ffd324; padding:10px; border-radius:6px; }
    .kbd { background:#eef2ff; border:1px solid #dbeafe; padding:4px 8px; border-radius:6px; display:inline-block; font-family: ui-monospace, monospace; }
    a { color:#0b6b9a; text-decoration: none; }
  </style>
</head>
<body>
  <main>
    <h1>ADGM Corporate Agent — README</h1>

    <section class="section">
      <h2>Overview</h2>
      <p>
        This Streamlit app is an AI-powered <strong>Corporate Agent</strong> for the Abu Dhabi Global Market (ADGM).
        It reviews uploaded <code>.docx</code> legal documents, verifies checklists for processes (e.g. Company Incorporation),
        flags legal red-flags, inserts inline review notes into the file, and produces a structured JSON report.
      </p>
    </section>

    <section class="section">
      <h2>Features</h2>
      <ul>
        <li><strong>Upload &amp; Parse</strong> — accepts <code>.docx</code> documents for review.</li>
        <li><strong>Process Detection</strong> — recognizes processes such as Company Incorporation using keywords.</li>
        <li><strong>Checklist Verification</strong> — compares uploaded docs with ADGM checklists and lists missing docs.</li>
        <li><strong>Red-Flag Detection</strong> — flags jurisdiction issues, missing signature blocks, ambiguous language, etc.</li>
        <li><strong>Inline Review Notes</strong> — inserts visible review notes (red inline tags) into the document.</li>
        <li><strong>RAG Integration</strong> — optionally uses uploaded ADGM reference documents and Gemini (Gemini 2.5 Flash) to augment findings and provide citations (falls back to rule-based checks if RAG fails).</li>
        <li><strong>Outputs</strong> — downloadable reviewed <code>.docx</code> and a structured JSON report.</li>
      </ul>
    </section>

    <section class="section">
      <h2>Requirements</h2>
      <ul>
        <li>Python 3.9+</li>
        <li>Virtual environment recommended</li>
        <li>Python packages: <code>streamlit</code>, <code>python-docx</code>, <code>google-genai</code>, <code>numpy</code></li>
      </ul>
    </section>

    <section class="section">
      <h2>Installation</h2>
      <pre><code># Create and activate a virtual environment
python -m venv venv
# mac / linux
source venv/bin/activate
# windows (PowerShell)
venv\Scripts\Activate

# Upgrade pip and install dependencies
pip install --upgrade pip
pip install streamlit python-docx google-genai numpy
</code></pre>
    </section>

    <section class="section">
      <h2>Environment Variables</h2>
      <p>Set your Gemini API key in the same shell you run Streamlit from.</p>
      <pre><code># mac / linux
export GEMINI_API_KEY="your_key_here"

# Windows PowerShell
$env:GEMINI_API_KEY="your_key_here"
</code></pre>
      <p class="note"><strong>Security note:</strong> Do not commit your API key to source control. Use environment variables only.</p>
    </section>

    <section class="section">
      <h2>Run the App</h2>
      <pre><code>streamlit run adgm_agent.py</code></pre>
      <p>The app will open in your default browser at <code>http://localhost:8501</code> (or the port Streamlit allocates).</p>
    </section>

    <section class="section">
      <h2>Usage</h2>
      <ol>
        <li>Optionally upload ADGM reference documents (<code>.docx</code> or <code>.txt</code>) and click <em>Build/Refresh RAG index</em> to enable RAG &amp; citations.</li>
        <li>Upload the user <code>.docx</code> files you want reviewed.</li>
        <li>If you uploaded references, build the index (embeddings will be created).</li>
        <li>The agent runs rule-based checks and (if available) RAG + Gemini to augment results.</li>
        <li>Download reviewed <code>.docx</code> files and the JSON report.</li>
      </ol>
    </section>

    <section class="section">
      <h2>JSON Output Example</h2>
      <pre><code>{
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
</code></pre>
    </section>

    <section class="section">
      <h2>Submission Checklist</h2>
      <ul>
        <li><code>adgm_agent.py</code> — main Streamlit app</li>
        <li><code>README.html</code> (this file)</li>
        <li>One example <code>.docx</code> before review and one after review</li>
        <li>Generated JSON report</li>
        <li>Optional: screenshot or short demo video</li>
      </ul>
    </section>

    <section class="section">
      <h2>Notes &amp; Limitations</h2>
      <ul>
        <li>RAG depends on the uploaded ADGM reference docs: better/more relevant references = better AI citations.</li>
        <li>If RAG/Gemini returns non-JSON or fails, the app falls back to rule-based checks and still returns outputs.</li>
        <li>Inline review notes are inserted as visible red inline tags (easily visible in Word). If you want Word comment balloons (native comment objects), that requires additional XML edits; ask and it can be added.</li>
      </ul>
    </section>

    <section class="section">
      <h2>Questions or Next Steps</h2>
      <p>If you want I can:</p>
      <ul>
        <li>Generate native Word comment balloons instead of inline tags</li>
        <li>Create example before/after files and the JSON report</li>
        <li>Bundle everything into a ZIP ready for upload</li>
      </ul>
    </section>

    <footer style="margin-top:24px;font-size:0.95em;color:#334155">
      <p>Prepared for ADGM document intelligence submission — good luck!</p>
    </footer>
  </main>
</body>
</html>
