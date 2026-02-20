# AeroDoc — RAG-Powered Aviation Technical Assistant

A Retrieval-Augmented Generation (RAG) system that allows aviation technicians and engineers to query FAA technical manuals and maintenance documentation using natural language. Instead of manually searching through hundreds of pages, users can ask plain-English questions and receive accurate, context-grounded answers.

---

## How It Works

AeroDoc uses a standard RAG pipeline built on LangChain and ChromaDB:

1. **Document Loading** — FAA PDF manuals are loaded using `PyPDFLoader`
2. **Chunking** — Documents are split using `RecursiveCharacterTextSplitter` with a chunk size of 800 and overlap of 150, tuned for technical documentation
3. **Embedding** — Chunks are embedded using OpenAI's `text-embedding-3-small` model
4. **Storage** — Embeddings are stored locally in a ChromaDB vector store
5. **Retrieval** — On query, the top-k most relevant chunks are retrieved via cosine similarity
6. **Generation** — Retrieved context is passed to `gpt-4o-mini` with a domain-specific prompt persona (professional aircraft technician)

---

## Tech Stack

| Component | Technology |
|-----------|------------|
| LLM | OpenAI GPT-4o-mini |
| Embeddings | OpenAI text-embedding-3-small |
| Vector Store | ChromaDB |
| RAG Framework | LangChain |
| Document Loader | PyPDFLoader |
| Language | Python 3.9+ |

---

## Project Structure

```
aerodoc/
│
├── docs/                          # FAA technical manuals (PDF)
│   └── FAA_Engine-Maintainance.pdf
│
├── notebooks/
│   └── rag_pipeline.ipynb         # Main RAG development notebook
│
├── src/
│   └── rag.py                     # Core RAG pipeline (modular)
│
├── requirements.txt
└── README.md
```

---

## Getting Started

### Prerequisites

- Python 3.9+
- OpenAI API key

### Installation

```bash
git clone https://github.com/mohd-mb2205153/aerodoc.git
cd aerodoc
pip install -r requirements.txt
```

### Environment Setup

```bash
export OPENAI_API_KEY=your_api_key_here
```

### Run

```bash
jupyter notebook notebooks/rag_pipeline.ipynb
```

---

## Requirements

```
langchain
langchain-openai
langchain-chroma
langchain-community
chromadb
pypdf
openai
```

---

## Key Design Decisions

**Chunk size of 800 with 150 overlap** — Technical documents like FAA manuals contain dense procedural information. A larger chunk size preserves procedural context (e.g., multi-step inspection procedures) while the 15-20% overlap prevents information loss at chunk boundaries.

**RecursiveCharacterTextSplitter** — Chosen over fixed-size splitting to respect natural text boundaries (paragraphs, sentences) and preserve semantic coherence within chunks.

**Domain-specific prompt persona** — The system prompt instructs the model to respond as a professional aircraft technician, improving the relevance and tone of answers for the target use case.

---

## Roadmap

- [ ] Streamlit web interface for non-technical users
- [ ] Multi-document support (ingest entire folder of FAA manuals)
- [ ] Conversation memory for follow-up questions
- [ ] Evaluation framework with test Q&A pairs from FAA documentation
- [ ] Hybrid search (keyword + semantic) for improved retrieval accuracy
- [ ] Semantic chunking experiments

---

## Limitations

- Currently supports single-document ingestion
- No conversation memory between queries (each query is independent)
- Retrieval quality has not been formally evaluated
- Requires an active OpenAI API key

---
