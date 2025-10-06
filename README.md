# EMITRR – AI Medical Interview Transcription & Reporting

End-to-end Jupyter notebook for extracting medical entities, summarizing dialogue, analyzing sentiment/intent, and generating a clinical-style SOAP note from physician–patient conversations.

---

## Features

- **Medical NER** with SciSpaCy + BC5CDR (`MedicalNER`)  
- **Summarization** into a structured JSON summary (`MedicalSummarizer`)  
- **Per-utterance sentiment & intent** analysis (`MedicalSentimentAnalyzer`)  
- **SOAP note generation** (`SOAPNoteGenerator`) – Subjective, Objective, Assessment, Plan  

---

## Repository Structure

.
├─ EMITRR_AIINTERN_VEDHAM.ipynb # Main notebook (end-to-end workflow & demo)

yaml
Copy code

---

## Quickstart

1. **Create & activate a virtual environment** (optional but recommended):
   ```bash
   python3 -m venv .venv
   source .venv/bin/activate
Upgrade basics:

bash
Copy code
python3 -m pip install --upgrade pip setuptools wheel
Install core dependencies:

bash
Copy code
python3 -m pip install spacy==3.6.1 scispacy==0.5.5 medspacy transformers torch datasets textblob pandas numpy
Install SciSpaCy models (requires spaCy >=3.6.1,<3.7.0):

bash
Copy code
python3 -m pip install https://s3-us-west-2.amazonaws.com/ai2-s2-scispacy/releases/v0.5.3/en_core_sci_sm-0.5.3.tar.gz

python3 -m pip install https://s3-us-west-2.amazonaws.com/ai2-s2-scispacy/releases/v0.5.3/en_ner_bc5cdr_md-0.5.3.tar.gz
Tip: You can add these dependencies to a requirements.txt later. Keep spaCy pinned to 3.6.1 because SciSpaCy v0.5.3 models require <3.7.0.

Run the notebook:

bash
Copy code
jupyter notebook EMITRR_AIINTERN_VEDHAM.ipynb
Then run all cells in order.

Notebook Workflow
Loads en_core_sci_sm and en_ner_bc5cdr_md (falls back gracefully if missing).

Parses example physician–patient dialogues.

Extracts entities and builds a structured summary (JSON).

Performs statement-level sentiment & intent analysis.

Generates a SOAP-style note (JSON) with metadata + S/O/A/P sections.

Expected Output (Example)
json
Copy code
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

bash
Copy code
python3 -m pip install --force-reinstall "spacy==3.6.1"
Model load failures (spacy.load('en_core_sci_sm') or en_ner_bc5cdr_md)
Re-install from the URLs above. The notebook will fall back gracefully, but results may be reduced.

Apple Silicon (M-series) + PyTorch
