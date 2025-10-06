# EMITR – AI Medical Interview Transcription & Reporting

A Jupyter notebook (`emitr.ipynb`) that processes physician–patient dialogues to:

- Extract medical entities with SciSpaCy/BC5CDR (`MedicalNER`).
- Summarize findings into a structured JSON summary (`MedicalSummarizer`).
- Analyze statement-level sentiment and intent (`MedicalSentimentAnalyzer`).
- Generate a clinical-style SOAP note (`SOAPNoteGenerator`).

---

## Contents

- `EMITRR_AIINTERN_VEDHAM.ipynb` – end-to-end workflow with modular classes and demo output.

Key classes defined in the notebook:
- `MedicalNER` – aggregates symptoms, diagnoses, treatments, medications, body parts, temporal, severity, etc.
- `MedicalSentimentAnalyzer` – TextBlob fallback and optional BERT pipeline mapping to 5-point sentiment; basic intent heuristics.
- `SOAPNoteGenerator` – produces a structured SOAP JSON using dialogue, entities, summaries, and sentiments.
---

## Setup

Tested with Python 3.11 on macOS.

### 1) Base packages

```bash
python3 -m pip install --upgrade pip setuptools wheel
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
```

Why spaCy 3.6.1? The SciSpaCy models used here require spaCy <3.7.0 and are compatible with 3.6.1. Mixing newer spaCy with older models triggers resolver warnings/conflicts.

### 2) SciSpaCy/BC5CDR models

Install the small SciSpaCy core model and the BC5CDR NER model used in the notebook:

```bash
# SciSpaCy small model
ython3 -m pip install \
  https://s3-us-west-2.amazonaws.com/ai2-s2-scispacy/releases/v0.5.3/en_core_sci_sm-0.5.3.tar.gz

# BC5CDR disease/chemical NER model
python3 -m pip install \
  https://s3-us-west-2.amazonaws.com/ai2-s2-scispacy/releases/v0.5.3/en_ner_bc5cdr_md-0.5.3.tar.gz
```

Notes:
- `en_core_sci_sm 0.5.3` requires `spacy>=3.6.1,<3.7.0`.
- `en_ner_bc5cdr_md 0.5.3` requires `spacy>=3.6.1,<3.7.0`.
- If either model is missing, the notebook falls back to `en_core_web_sm` or skips BC5CDR gracefully.

### 3) Optional models

If you want BERT-based sentiment via `transformers`, the default pipeline will be downloaded the first time you run it (internet required). If unavailable, the notebook uses TextBlob.

---

## Running the Notebook

1. Launch Jupyter and open the notebook:
   ```bash
   jupyter notebook EMITRR_AIINTERN_VEDHAM.ipynb
   ```
2. Run all cells in order. The notebook:
   - Loads models (`spacy.load('en_core_sci_sm')`, `spacy.load('en_ner_bc5cdr_md')`, `medspacy.load()`).
   - Parses example physician/patient dialogue (`dialogue_data`).
   - Extracts entities and builds a structured medical summary (JSON).
   - Performs sentiment/intent analysis for each statement.
   - Generates and prints a SOAP note with metadata, Subjective, Objective, Assessment, and Plan sections.

Expected outputs include sections like:
- “Medical Entities Extracted” with categories (DIAGNOSES, SYMPTOMS, TREATMENTS, BODY_PARTS, TEMPORAL, SEVERITY).
- “SENTIMENT ANALYSIS RESULTS” with statement-level scores and an overall summary.
- “SOAP NOTE GENERATED” showing a structured JSON note.

---

## Troubleshooting

- SpaCy version conflicts: If you see messages like “requires spacy<3.7.0,>=3.6.1”, make sure you’re on spaCy 3.6.1 before installing the models. You can force it with:
  ```bash
  python3 -m pip install --force-reinstall "spacy==3.6.1"
  ```
- Missing models: If `spacy.load('en_core_sci_sm')` or `spacy.load('en_ner_bc5cdr_md')` fails, install from the URLs above. The notebook will fallback when possible but results may be reduced.
- Apple Silicon (M-series) and PyTorch: The generic `torch` wheel should work; for best performance, consult the PyTorch site for MPS-accelerated builds.

---


