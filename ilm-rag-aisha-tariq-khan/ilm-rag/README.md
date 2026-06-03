# ILM RAG — RAG Document Intelligence

> **Built by Aisha Tariq Khan** for legal document analysis using Retrieval-Augmented Generation (RAG).

A production-ready tool that ingests 500-page legal contracts and:
- ✅ Answers questions with **citation-enforced** responses (never hallucinates)
- ✅ Extracts **Risks**, **Dates**, and **Stakeholders** automatically
- ✅ Shows every page number and section that supports each answer
- ✅ Works entirely in the browser — no backend required

---

## Live Demo

Click **"Choose File"** to upload any PDF contract, then:
1. Ask natural language questions in the **Ask Document** tab
2. Click **"Run Analysis"** to populate the **Summary Dashboard**

---

## RAG Architecture

```
PDF Upload
    │
    ▼
PDF.js Text Extraction  ←── page-by-page extraction
    │
    ▼
Chunker                 ←── 1500-char chunks, 200-char overlap
    │
    ▼
Keyword Index           ←── TF-IDF-style keyword scoring
    │
    ▼
Query → Retrieve        ←── Top-8 chunks retrieved per query
    │
    ▼
Context + Prompt        ←── Retrieved chunks injected into Claude's context
    │
    ▼
Claude (claude-sonnet-4-20250514)
    │
    ▼
Citation-Grounded Answer  ←── [§X.X, p.N] enforced by system prompt
```

**Key Anti-Hallucination Technique:** The system prompt instructs Claude:
> "You may ONLY use information that appears in the DOCUMENT CONTEXT below.
> Every factual statement MUST be followed by a citation [p.N]."

---

## Setup & Deployment

### Option 1: GitHub Pages (free, static)

1. Fork this repository
2. Go to **Settings → Pages → Source: main branch / root**
3. Open `js/app.js` and update the API proxy URL (see Option 3 below)
4. Your app is live at `https://yourusername.github.io/ilm-rag/`

> ⚠️ **Important:** The Anthropic API requires a backend proxy for browser apps since API keys must never be exposed client-side. See Option 3 for the proxy setup.

### Option 2: Local Development

```bash
# Clone the repo
git clone https://github.com/yourname/ilm-rag.git
cd ilm-rag

# Serve locally (any static server works)
npx serve .
# or
python -m http.server 8080
```

Open `http://localhost:8080`

### Option 3: API Proxy (Required for Production)

Never expose your API key in browser code. Set up a lightweight proxy:

#### Using Cloudflare Workers (free tier)

```javascript
// worker.js
export default {
  async fetch(request, env) {
    if (request.method === 'OPTIONS') {
      return new Response(null, {
        headers: {
          'Access-Control-Allow-Origin': 'https://yourdomain.com',
          'Access-Control-Allow-Headers': 'Content-Type',
          'Access-Control-Allow-Methods': 'POST'
        }
      });
    }

    const body = await request.json();
    const response = await fetch('https://api.anthropic.com/v1/messages', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'x-api-key': env.ANTHROPIC_API_KEY,   // set in Cloudflare dashboard
        'anthropic-version': '2023-06-01'
      },
      body: JSON.stringify(body)
    });

    const data = await response.json();
    return new Response(JSON.stringify(data), {
      headers: { 'Access-Control-Allow-Origin': '*', 'Content-Type': 'application/json' }
    });
  }
};
```

Deploy with: `wrangler deploy`

Then update `js/app.js`:
```js
// Replace:
const response = await fetch('https://api.anthropic.com/v1/messages', { ... });
// With:
const response = await fetch('https://your-worker.workers.dev/proxy', { ... });
```

#### Using Express.js (Node.js)

```bash
npm install express cors @anthropic-ai/sdk
```

```javascript
// server.js
const express = require('express');
const cors = require('cors');
const Anthropic = require('@anthropic-ai/sdk');

const app = express();
const client = new Anthropic({ apiKey: process.env.ANTHROPIC_API_KEY });

app.use(cors({ origin: process.env.ALLOWED_ORIGIN }));
app.use(express.json({ limit: '10mb' }));

app.post('/api/chat', async (req, res) => {
  try {
    const message = await client.messages.create(req.body);
    res.json(message);
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});

app.listen(3000, () => console.log('Proxy running on port 3000'));
```

```bash
ANTHROPIC_API_KEY=sk-ant-... ALLOWED_ORIGIN=https://yourdomain.com node server.js
```

---

## Project Structure

```
ilm-rag/
├── index.html          # Main app shell
├── css/
│   └── style.css       # All styles
├── js/
│   └── app.js          # RAG logic + Claude API calls
└── README.md           # This file
```

---

## Features

| Feature | Implementation |
|---|---|
| PDF parsing | pdf.js (client-side, no upload to server) |
| Chunking | Sliding window, 1500 chars, 200 overlap |
| Retrieval | Keyword TF-IDF scoring, Top-8 chunks |
| LLM | Claude Sonnet 4 via Anthropic API |
| Citation enforcement | System prompt + regex extraction |
| Anti-hallucination | Context-only answering + explicit refusal |
| Dashboard | Structured JSON extraction prompt |
| Deployment | Pure static HTML/CSS/JS |

---

## Prompt Engineering (RAG System Prompt)

The core technique that prevents hallucination:

```
CRITICAL RAG RULES:
1. Every factual statement MUST end with [p.N] or [§X.X, p.N]
2. You may ONLY use information from the DOCUMENT CONTEXT below
3. If the answer is not in the context, say so explicitly
4. Never invent page numbers, clause numbers, or dates
```

---

## License

MIT © 2024 Aisha Tariq Khan
