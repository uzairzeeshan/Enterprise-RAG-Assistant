# Enterprise Multi-Source RAG Assistant 🚀

An enterprise-grade **Retrieval-Augmented Generation (RAG)** platform designed to ingest multi-source documents (**PDF, DOCX, PPTX, CSV, Web URLs**), perform **Hybrid Retrieval (BM25 + Vector Search)**, rerank passages using **Cross-Encoders**, enforce **Role-Based Access Control (RBAC)**, generate answers with **source citations**, and present an interactive **Streamlit Dashboard** and **FastAPI REST API**.

---

## 🌟 Key Features

1. **Multi-Source Ingestion Engine**:
   - Parses **PDFs** (page-by-page), **Word Documents** (`.docx`), **PowerPoint** (`.pptx`), tabular **CSVs**, and **Web Pages** (HTTP scraping).
   - Generates metadata-enriched text chunks with source file, page/slide/section, and access level flags.

2. **Advanced Hybrid Retrieval**:
   - **Dense Search**: Persistent **ChromaDB** vector store with Sentence-Transformer embeddings (`all-MiniLM-L6-v2`).
   - **Sparse Search**: **BM25 Keyword Search** engine (`rank-bm25`).
   - **Reciprocal Rank Fusion (RRF)**: Merges sparse and dense search scores with customizable weight ($\alpha$).

3. **Cross-Encoder Reranking**:
   - Reranks candidate passages using a cross-encoder model (`ms-marco-MiniLM-L-6-v2`) to maximize context relevance before feeding to the LLM.

4. **Role-Based Access Control (RBAC)**:
   - Three tier roles: `admin`, `analyst`, and `user`.
   - Document access levels: `public`, `confidential`, and `admin_only`.
   - Automatic query filtering so users only retrieve documents within their permission tier.

5. **Multi-Provider LLM & Source Citations**:
   - Answer synthesis with explicit inline citations: `[Doc 1: filename.pdf, Page 3]`.
   - Supports **Google Gemini**, **OpenAI GPT**, or zero-setup **Local Mock Mode** out of the box.

6. **Enterprise Admin & Security Audit Console**:
   - Audit trail logging (`audit_logs` table for queries, uploads, logins, and deletions).
   - User account creation and management.
   - Vector store and keyword index synchronization.
   - Real-time performance analytics with Plotly charts.

7. **Production Deployment**:
   - Dockerized backend (`Dockerfile.backend`) and frontend (`Dockerfile.frontend`) managed via `docker-compose.yml`.

---

## 🛠 Tech Stack

- **Backend**: FastAPI, Python 3.11, Pydantic v2, SQLAlchemy, Uvicorn
- **RAG & ML**: ChromaDB, rank-bm25, Sentence-Transformers, PyPDF, python-docx, python-pptx, pandas, BeautifulSoup4
- **Security & DB**: JWT (python-jose), Passlib (Bcrypt), SQLite / PostgreSQL
- **Frontend**: Streamlit, Plotly, Extra-Streamlit-Components, Custom Glassmorphic CSS
- **Deployment**: Docker, Docker Compose

---

## 🚀 Quick Start (Local Setup)

### 1. Clone & Set Up Environment

```bash
git clone <your-repo-url>
cd enterprise-rag-assistant

# Create virtual environment
python -m venv venv
# On Windows:
venv\Scripts\activate
# On Linux/Mac:
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt
```

### 2. Configure Environment Variables

```bash
cp .env.example .env
```
*(Optionally set `GEMINI_API_KEY` or `OPENAI_API_KEY` in `.env`. By default `LLM_PROVIDER="mock"` works offline with zero API key required!)*

### 3. Run Backend API

```bash
uvicorn app.main:app --reload --port 8000
```
- API Docs available at: [http://localhost:8000/docs](http://localhost:8000/docs)

### 4. Run Streamlit Frontend

In a second terminal:

```bash
streamlit run frontend/app.py
```
- Open UI at: [http://localhost:8501](http://localhost:8501)
- Default Admin Credentials:
  - **Username**: `admin`
  - **Password**: `adminpassword123`

---

## 🐳 Docker Setup

Run the entire stack with Docker Compose:

```bash
docker-compose up --build
```

- Backend API: [http://localhost:8000](http://localhost:8000)
- Streamlit UI: [http://localhost:8501](http://localhost:8501)

---

## 🧪 Running Automated Tests

Run unit and integration tests with `pytest`:

```bash
pytest -v
```

---

## 💼 Resume Bullet Points

Include this project on your resume to showcase high-impact enterprise AI engineering:

- **Built an Enterprise Multi-Source RAG System** using FastAPI, ChromaDB, and Streamlit, enabling semantic search across 5 file formats (PDF, DOCX, PPTX, CSV, Web URLs).
- **Designed a Hybrid Retrieval Engine** combining BM25 keyword search and dense vector embeddings using Reciprocal Rank Fusion (RRF) and Cross-Encoder reranking, improving retrieval precision.
- **Implemented Granular Role-Based Access Control (RBAC)** and JWT authentication, ensuring document privacy across multi-tenant user roles (`admin`, `analyst`, `user`).
- **Engineered an Audit Logging & Analytics Pipeline** tracking query latency, document chunk statistics, and user activity, deployed via Docker Compose.
