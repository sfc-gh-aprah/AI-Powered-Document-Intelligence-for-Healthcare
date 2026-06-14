# AI-Powered Document Intelligence for Healthcare

Turn unstructured clinical documents into searchable, queryable data — **entirely inside Snowflake**, with no external OCR vendor, no model training, and no data leaving your governed, HIPAA-eligible environment.

This is the hands-on companion to the **AI-Powered Document Intelligence for Healthcare** virtual lab. Clone it and run alongside the session, or work through it on your own afterward.

> **All sample documents are 100% synthetic.** Patient names, providers, facilities, and payers are entirely fabricated (placeholders like "Doe," "Roe," "Sample Hospital," "Sample Insurance Company"). No real patient information is used.

---

## What you'll build

A complete document-intelligence pipeline in five steps:

1. **Stage & ingest** — load clinical PDFs into a governed Snowflake stage
2. **Parse** — `AI_PARSE_DOCUMENT` extracts text, tables, and layout
3. **Search** — a Cortex Search service enables semantic (meaning-based) retrieval
4. **Ask** — a Snowflake Intelligence / CoWork agent answers natural-language questions with citations
5. **Automate** — a scheduled task parses and indexes new documents automatically

---

## What's in this repo

```
clinical_doc_intelligence.ipynb   The notebook — import this into Snowflake
sample_documents/                 6 synthetic clinical PDFs (loaded in Step 1)
  ├─ discharge_summary_doe_anna.pdf
  ├─ discharge_summary_roe_thomas.pdf
  ├─ lab_report_doe_anna.pdf
  ├─ prior_auth_form_knee_replacement.pdf
  ├─ prior_auth_form_cardiac_rehab.pdf
  └─ clinical_note_progress_doe.pdf
new_arrival/                      1 extra PDF for the Step 5 automation demo
  └─ discharge_summary_poe_sarah.pdf
```

The documents tell two connected patient stories so the search and agent demos are compelling:
- **Anna Doe** (heart failure): lab report, discharge summary, an approved cardiac-rehab prior auth, and a follow-up progress note — four documents from four different source systems.
- **Thomas Roe** (knee replacement): a prior authorization request and a surgical discharge summary.

---

## Prerequisites

- A Snowflake account with **Cortex AI** available in your region.
- A role with privileges to create a database, schema, and warehouse.
- The **`SNOWFLAKE.CORTEX_USER`** database role (required for `AI_PARSE_DOCUMENT`, `AI_EXTRACT`, and Cortex Search).
- For Step 4 (the agent): the **`CREATE AGENT`** privilege (e.g., via `SNOWFLAKE_INTELLIGENCE_ADMIN`).

---

## Setup

### 1. Import the notebook
In Snowsight: **Projects → Notebooks → ⋯ (top-right) → Import .ipynb file**, and select `clinical_doc_intelligence.ipynb`. Choose a database, schema, and warehouse to host the notebook (it will create its own demo objects when you run it).

### 2. Load the sample documents
The notebook's **Segment 1** loads the PDFs straight from this repo using Snowflake's Git integration — just run the cells in order. No manual upload needed.

**If Git integration isn't enabled in your account**, you can upload the files manually instead:
Snowsight → **Data → Databases → `HEALTHCARE_DOC_AI` → `CLINICAL_DOCS` → Stages → `CLINICAL_DOCS_STAGE` → + Files**, then upload everything in `sample_documents/`.

### 3. Run the notebook top to bottom
Each section has a markdown explainer followed by the cell that does the work. Run them in order.

### 4. Build the agent (Step 4 — done in the UI)
Agent creation is a few clicks in Snowsight, not code. The notebook's Segment 4 has the exact steps:
- **AI & ML → Agents → Create agent** → *Create this agent for Snowflake Intelligence*
- Name it `CLINICAL_DOCS_AGENT`, display name **Clinical Document Assistant**
- Add the **Cortex Search** tool → select `CLINICAL_DOC_SEARCH`
- Set the response instruction to require citations
- Surface it under **Agents → Snowflake Intelligence tab → settings**

Then ask:
> *Summarize Anna Doe's heart failure hospitalization and what changed at her follow-up visit.*
>
> *What conservative treatments did Thomas Roe fail before his knee replacement was authorized?*

### 5. See automation in action (Step 5)
Upload `new_arrival/discharge_summary_poe_sarah.pdf` to `CLINICAL_DOCS_STAGE` (this document is deliberately kept out of the Step 1 load), then run the task cells. The new document is parsed, indexed, and answerable — no manual steps.

---

## Naming note

Snowflake announced at Summit 2026 that **Snowflake Intelligence is becoming Snowflake CoWork**. The capability is the same; depending on when you run this, your UI may show either name.

---

## Try it on your own documents

Take ten de-identified documents from your own repository, point this same pattern at them, and see what comes back. The architecture doesn't change — only the input does.

---

*Sample data is synthetic and provided for demonstration only. This repository is an educational example, not medical or compliance advice.*
