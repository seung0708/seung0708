# Sentinel AI 🛡️

A collaborative fraud detection platform combining ensemble machine learning with a natural language query interface. Built with a team to give merchants an intuitive way to investigate fraud signals without writing SQL.

**[Live Demo](https://your-demo-link.vercel.app)** · **[API Docs](https://your-demo-link.vercel.app/docs)**

> **Collaboration note:** Sentinel AI was built as a collaborative project. My primary contributions were the Next.js frontend and dashboard, the conversational query interface (LangChain + LlamaIndex + pgvector pipeline), and the Supabase integration layer. The XGBoost modeling pipeline and Flask API were built collaboratively with my teammates.

---

## Overview

Sentinel AI gives merchants a two-layer fraud detection system: a machine learning backend that classifies transactions in real time, and a natural language interface that lets non-technical users investigate fraud patterns by asking plain English questions.

Instead of querying a database or reading raw model outputs, a merchant can ask *"Which high-risk transactions came from new accounts in the last 7 days?"* and get a structured, human-readable answer backed by semantic search over transaction embeddings.

---

## Features

### ML Risk Classification
- Three specialized XGBoost models handle multi-class risk classification (low / medium / high)
- Models are trained on transaction features including velocity, behavioral signals, and account history
- Flask API serves real-time predictions with risk scores and feature attribution

### Conversational Query Interface
- Natural language questions are converted to semantic search queries using OpenAI embeddings
- pgvector similarity search retrieves relevant transactions from Supabase
- LangChain orchestrates the retrieval-augmented generation (RAG) pipeline
- LlamaIndex structures the document context fed to the language model

### Merchant Dashboard
- Transaction feed with live risk score overlays
- Filter and drill into flagged transactions by risk tier, date, and merchant category
- Conversational query panel embedded directly in the review workflow

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Next.js 14, React, TypeScript, Tailwind CSS |
| Conversational AI | LangChain, LlamaIndex, OpenAI API |
| ML Models | XGBoost, Scikit-learn, Python |
| Model Serving | Flask REST API |
| Vector Search | pgvector (PostgreSQL extension via Supabase) |
| Database | PostgreSQL / Supabase |
| Embeddings | OpenAI `text-embedding-ada-002` |
| Deployment | Vercel (frontend), Railway (Flask API) |

---

## Architecture

```
sentinel-ai/
├── frontend/                   # Next.js dashboard
│   ├── app/
│   │   ├── dashboard/          # Transaction feed + risk overlays
│   │   ├── investigate/        # Conversational query interface
│   │   └── api/                # Next.js route handlers
│   ├── components/
│   │   ├── TransactionTable/
│   │   ├── RiskBadge/
│   │   └── QueryChat/          # NL query UI component
│   └── lib/
│       ├── supabase/
│       └── langchain/          # RAG pipeline client
│
├── ml/                         # Python ML pipeline (collaborative)
│   ├── models/
│   │   ├── risk_classifier.py  # XGBoost training + inference
│   │   └── feature_engineering.py
│   ├── api/
│   │   └── app.py              # Flask prediction API
│   └── notebooks/              # EDA and model evaluation
│
└── supabase/
    └── migrations/             # Schema + pgvector setup
```

---

## Conversational Query Pipeline

The natural language interface uses a RAG architecture to answer merchant questions grounded in actual transaction data.

```
User question
     │
     ▼
OpenAI Embeddings (text-embedding-ada-002)
     │
     ▼
pgvector similarity search → top-k relevant transactions
     │
     ▼
LlamaIndex document structuring
     │
     ▼
LangChain prompt assembly + OpenAI completion
     │
     ▼
Structured answer with cited transactions
```

**Example queries the system handles:**
- *"Show me high-risk transactions from accounts created in the last 30 days"*
- *"Which merchants had the most fraud flags this week?"*
- *"What patterns do the declined transactions from yesterday share?"*

---

## ML Model Details

Three XGBoost models classify transaction risk across different fraud typologies:

| Model | Target | Key Features |
|---|---|---|
| Velocity Model | Rapid successive transactions | Transaction frequency, time deltas |
| Account Model | New/anomalous account behavior | Account age, login patterns |
| Merchant Model | Category-level risk signals | MCC codes, merchant history |

Final risk classification is an ensemble of all three model outputs.

---

## Getting Started

### Prerequisites
- Node.js 18+
- Python 3.10+
- Supabase project with pgvector enabled
- OpenAI API key

### Installation

```bash
git clone https://github.com/yourusername/sentinel-ai.git
cd sentinel-ai
```

**Frontend:**
```bash
cd frontend
npm install
```

**ML API:**
```bash
cd ml
pip install -r requirements.txt
```

### Environment Variables

```env
# frontend/.env.local
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
OPENAI_API_KEY=
ML_API_URL=http://localhost:5000
```

### Run Locally

```bash
# Terminal 1 — Flask ML API
cd ml && python api/app.py

# Terminal 2 — Next.js frontend
cd frontend && npm run dev
```

---

## Contributors

Built collaboratively by a team of [N] engineers.

| Contributor | GitHub |
|---|---|
| Your Name | [@yourusername](https://github.com/yourusername) |
| Teammate Name | [@teammateusername](https://github.com/teammateusername) |

---

## License

MIT
