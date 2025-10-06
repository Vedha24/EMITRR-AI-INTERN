EMITRR – AI Medical Interview Transcription & Reporting

End-to-end Jupyter notebook for extracting medical entities, summarizing dialogue, analyzing sentiment/intent, and generating a clinical-style SOAP note from physician–patient conversations.

Features

Medical NER with SciSpaCy + BC5CDR (MedicalNER)

Summarization into a structured JSON summary (MedicalSummarizer)

Per-utterance sentiment & intent (MedicalSentimentAnalyzer)

SOAP note generation (SOAPNoteGenerator) – Subjective, Objective, Assessment, Plan

Repository Structure
.
├─ EMITRR_AIINTERN_VEDHAM.ipynb   # main notebook (end-to-end workflow & demo)


Quickstart
# 1) Create & activate a virtual env (optional but recommended)
python3 -m venv .venv && source .venv/bin/activate

# 2) Upgrade basics
python3 -m pip install --upgrade pip setuptools wheel

# 3) Install core dependencies
python3 -m pip install \
  spacy==3.6.1 \
  scispacy==0.5.5 \
  medspacy \
  transformers \
  torch \
  datasets \
  textblob \
  pandas \
  numpy

# 4) Install SciSpaCy models (require spaCy >=3.6.1,<3.7.0)
python3 -m pip install \
  https://s3-us-west-2.amazonaws.com/ai2-s2-scispacy/releases/v0.5.3/en_core_sci_sm-0.5.3.tar.gz

python3 -m pip install \
  https://s3-us-west-2.amazonaws.com/ai2-s2-scispacy/releases/v0.5.3/en_ner_bc5cdr_md-0.5.3.tar.gz


If you prefer, you can add these to a requirements.txt later. Keep spaCy pinned to 3.6.1 because the 0.5.3 SciSpaCy models depend on <3.7.0.

Run
jupyter notebook EMITRR_AIINTERN_VEDHAM.ipynb
# then Run All cells in order


What the notebook does:

Loads en_core_sci_sm and en_ner_bc5cdr_md (falls back gracefully if missing).

Parses example physician–patient dialogue.

Extracts entities and builds a structured summary (JSON).

Performs statement-level sentiment & intent analysis.

Emits a SOAP-style note (JSON) with metadata + S/O/A/P.

Expected Output (snippet)
{
  "soap_note": {
    "Subjective": "...",
    "Objective": "...",
    "Assessment": ["..."],
    "Plan": ["..."]
  },
  "entities": {
    "DIAGNOSES": [...],
    "SYMPTOMS": [...],
    "TREATMENTS": [...]
  },
  "sentiment_summary": {
    "overall": "neutral",
    "per_statement": [...]
  }
}

Troubleshooting

spaCy version conflict
If you see: requires spacy<3.7.0,>=3.6.1

python3 -m pip install --force-reinstall "spacy==3.6.1"


Model load failures (spacy.load('en_core_sci_sm') / en_ner_bc5cdr_md)
Re-install from the URLs above. The notebook will fall back when possible, but results may be reduced.

Apple Silicon (M-series) + PyTorch
The generic torch wheel works; for best performance, consult PyTorch docs for MPS builds.

Notes

Large notebooks: If EMITRR_AIINTERN_VEDHAM.ipynb ≥ 100 MB, use Git LFS:

git lfs install
git lfs track "*.ipynb"
git add .gitattributes EMITRR_AIINTERN_VEDHAM.ipynb
git commit -m "Track notebook with LFS"


.gitignore (optional)

.ipynb_checkpoints/
__pycache__/
*.pyc
.venv/

Roadmap (optional)

Replace TextBlob fallback with a clinical sentiment model

Add evaluation scripts & unit tests

Package the pipeline as a CLI

Contributing

Issues and PRs are welcome. Please open an issue first for major changes.

License

MIT (or your preferred license).

Citation

If you use this project in academic work, please cite this repository.

Disclaimer

This project is for research/education only. Do not use outputs for clinical decision-making. Avoid including any PHI/PII in data or examples.
