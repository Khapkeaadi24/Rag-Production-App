📚 RAG Production App (Python)

A production-style Retrieval-Augmented Generation (RAG) application built with Python.
This project ingests PDFs, generates embeddings, stores them in Qdrant, orchestrates background jobs using Inngest, and provides a simple Streamlit UI for interaction.

🚀 Features

📄 PDF ingestion & chunking

🧠 Embeddings pipeline (local embeddings – no OpenAI dependency)

🗄️ Vector storage using Qdrant

⚙️ Background job orchestration with Inngest

🌐 Streamlit-based UI

🧩 Modular & production-friendly structure

🏗️ Project Structure
PythonProject1/
│
├── .venv/                  # Python virtual environment
├── qdrant_storage/         # Persistent Qdrant storage (Docker volume)
│   ├── aliases/
│   ├── collections/
│   ├── raft_state.json
│   └── .qdrant_fs_check
│
├── data_loader.py          # PDF loading, chunking & embedding logic
├── vector_db.py            # Qdrant client & vector DB operations
├── main.py                 # Core ingestion / orchestration logic
├── streamlit_app.py        # Streamlit UI
├── custom_types.py         # Shared type definitions
│
├── pyproject.toml          # Project metadata & dependencies
├── uv.lock                 # Dependency lockfile (uv)
├── .env                    # Environment variables
├── .gitignore
├── .python-version
└── README.md

🧠 Architecture Overview
PDF → Chunking → Embeddings → Qdrant
                 ↓
             Inngest
                 ↓
            Streamlit UI


Embeddings: Generated locally using SentenceTransformers

Vector DB: Qdrant (Docker)

Background Jobs: Inngest

UI: Streamlit

LLM (optional): Groq (for generation, not embeddings)

⚙️ Prerequisites

Make sure you have the following installed:

Python 3.10+

Docker Desktop (running, Linux containers)

Node.js (LTS) – required for Inngest CLI

pip / uv

🧪 Environment Setup
1️⃣ Create & activate virtual environment
python -m venv .venv
.venv\Scripts\activate

2️⃣ Install dependencies
pip install -r requirements.txt


or (if using uv):

uv sync

3️⃣ Environment variables (.env)
INNGEST_API_BASE=http://127.0.0.1:8288


(No OpenAI key required if using local embeddings)

🗄️ Start Qdrant (Docker)
docker run -d --name qdrantRagDb `
  -p 6333:6333 `
  -v ${PWD}\qdrant_storage:/qdrant/storage `
  qdrant/qdrant


Verify:

http://localhost:6333

⚡ Start Inngest Dev Server
npx inngest-cli@latest dev `
  -u http://127.0.0.1:8000/api/inngest `
  --no-discovery

🌐 Run Streamlit App

From the project root:

streamlit run streamlit_app.py


Open in browser:

http://localhost:8501

📄 PDF Ingestion Flow

User provides a PDF path (via UI or event)

data_loader.py:

Loads PDF

Splits text into chunks

Generates embeddings

vector_db.py:

Stores vectors in Qdrant

Inngest:

Handles ingestion as background job

Streamlit:

Displays status & enables querying

🧠 Embeddings Strategy

Local embeddings (SentenceTransformers)

No external API dependency

Faster & cheaper for production

Compatible with Qdrant

✅ Why this is “Production-Style”

Background processing (Inngest)

Persistent vector storage (Qdrant)

Clean modular codebase

UI decoupled from ingestion

No hard dependency on OpenAI

🛠️ Future Improvements

🔍 Hybrid search (BM25 + vector)

📊 Metadata-based filtering

🔐 Auth for Streamlit UI

🧪 Automated tests

☁️ Cloud deployment