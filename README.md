# 📚 RAG Document Intelligence Dashboard
### Task 3: The Knowledge Analyst — RAG Concepts
**Author:** Aisha Tariq Khan

---

## 🎯 Overview

A fully functional **Retrieval-Augmented Generation (RAG)** document intelligence tool that simulates how law firms and institutions can instantly query long documents — with **zero hallucination**, because every answer is grounded in cited page numbers and sections.

Built on the academic paper:
> *"The Impact of Artificial Intelligence (AI) on Students' Academic Development"*
> Vieriu & Petrea (2025) — *Education Sciences, 15(3), 343*

---

## ✨ Features

| Feature | Description |
|---|---|
| 📊 **Summary Dashboard** | Auto-extracts **Risks**, **Dates**, and **Stakeholders** from the document |
| 💬 **Cited Q&A** | Every answer cites `[Page X, Section: Y]` — no hallucination |
| 🔄 **RAG Workflow Visualizer** | Step-by-step pipeline: Chunking → Embedding → Retrieval → Generation |
| ✍️ **Prompt Library** | Engineered prompts for anti-hallucination, extraction, and validation |
| ⚡ **Confidence Scoring** | Each answer includes a grounding confidence percentage |

---

## 🚀 Deploy to GitHub Pages

### Step 1: Create Repository
```bash
git init
git add .
git commit -m "feat: RAG Document Intelligence Dashboard — Task 3"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/rag-document-intelligence.git
git push -u origin main
```

### Step 2: Enable GitHub Pages
1. Go to your repo → **Settings** → **Pages**
2. Source: **Deploy from a branch**
3. Branch: `main` / `root`
4. Click **Save**

### Step 3: Access Live URL
```
https://YOUR_USERNAME.github.io/rag-document-intelligence/
```

---

## 🔧 Local Development

No build step required — pure HTML/CSS/JS with React via CDN.

```bash
# Option 1: Python
python -m http.server 8080

# Option 2: Node
npx serve .

# Option 3: VS Code Live Server
# Right-click index.html → Open with Live Server
```

Open `http://localhost:8080` in your browser.

---

## 🏗️ RAG Pipeline Architecture

```
PDF Document
     │
     ▼
[1] CHUNKING ──── Split by section/page with metadata
     │
     ▼
[2] EMBEDDING ─── Encode chunks → vector space
     │
     ▼
[3] INDEX ──────── Store in vector DB (Pinecone/FAISS/Chroma)
     │
     ▼  ◄── User Question (embedded)
[4] RETRIEVAL ──── Top-K semantic search → relevant chunks only
     │
     ▼
[5] GENERATION ─── LLM + strict citation prompt → grounded answer
     │
     ▼
[6] RENDER ──────── Inline citations + confidence score
```

---

## 📐 Prompt Engineering Strategy

### Anti-Hallucination System Prompt
Forces the model to:
- Only use provided document chunks
- Cite `[Page X, Section: Y]` for every fact
- Explicitly say "not found" when information is absent
- End every response with a `CITATIONS USED` audit block

### Extraction Prompt
Automatically extracts structured data:
- **Risks** — Challenges, limitations, negative outcomes
- **Dates** — Publication timeline, study periods
- **Stakeholders** — Authors, institutions, participants

---

## 📁 File Structure

```
rag-document-intelligence/
├── index.html          # Full application (self-contained)
├── README.md           # This file
├── .github/
│   └── workflows/
│       └── deploy.yml  # Auto-deploy to GitHub Pages
└── docs/
    └── task3-report.md # Written task report
```

---

## 🤖 Technology Stack

- **Frontend:** React 18 (CDN), IBM Plex fonts, vanilla CSS
- **AI Backend:** Anthropic Claude API (`claude-sonnet-4-20250514`)
- **RAG Simulation:** Document chunked by section/page, semantic keyword matching for retrieval
- **Deployment:** GitHub Pages (static hosting, zero config)

---

## 📋 Task Requirements Checklist

- [x] Feed a long technical document into an AI model
- [x] Engineer prompts that force citation of page numbers/sections
- [x] Create a Summary Dashboard extracting Risks, Dates, Stakeholders
- [x] Simulate full RAG workflow
- [x] Zero hallucination — all answers grounded in document text
- [x] Ready to deploy on GitHub Pages

---

*Submitted by: **Aisha Tariq Khan** | Task 3: The Knowledge Analyst (RAG Concepts)*
