# Task 3 Report: The Knowledge Analyst (RAG Concepts)
**Author:** Aisha Tariq Khan

## Document Used
"The Impact of AI on Students' Academic Development" — Vieriu & Petrea (2025), Education Sciences 15(3), 343.

## What RAG Solves
Standard LLMs hallucinate facts. RAG grounds every answer in retrieved document chunks, forcing citation of page numbers. Critical for law firms querying contracts.

## Prompt Engineering
System prompt enforces:
1. Only answer from provided chunks
2. Cite [Page X, Section: Y] for every fact
3. Admit "not found" when outside scope
4. End with CITATIONS USED block

## Dashboard Extraction
Auto-extracts: Risks (5 items), Dates (5 items), Stakeholders (6 items) — all with page citations.

## RAG Pipeline
Chunk → Embed → Index → Retrieve → Generate (cited) → Render

Submitted by: Aisha Tariq Khan | Task 3: The Knowledge Analyst
