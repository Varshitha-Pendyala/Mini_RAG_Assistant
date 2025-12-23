# Mini_RAG_Assistant

A simple **Retrieval-Augmented Generation (RAG)** web application built with **Streamlit**.  
Ask questions about your documents — stored either **locally** (uploaded files) or in **Azure Blob Storage**.

### Features
- Toggle between **Local** and **Cloud (Azure Blob Storage)** data sources
- Upload PDF, DOCX, TXT files directly in the app (Local mode)
- Secure credential handling via `.env` or `RAG.env`
- OpenAI embeddings (`text-embedding-3-small`) and LLM (`gpt-4o-mini`)
- Conversation memory (last 4 turns)
- Retrieval quality metrics: Precision@3, Hit Rate@3, Faithfulness, Confidence Score
- Highlighted best-matching retrieved chunk
- Clean dark-themed UI

User → Streamlit UI → Document Upload → Text Extraction → Chunking → Embedding → FAISS Vector Store
Query → Embedding → Top-K Retrieval → LLM (GPT-4o-mini) → Answer + Confidence Score → Display

- Front-end: Streamlit interface for uploads and queries
- Backend: FAISS for semantic search, OpenAI embeddings for vectorization
- LLM: GPT-4o-mini generates answers strictly from retrieved chunks

### Retrieval & Generation Flow
- Document Upload: Users upload PDFs, DOCX, or TXT files.
- Text Extraction & Chunking: Large documents are split into smaller chunks using RecursiveCharacterTextSplitter.
- Embedding: Each chunk is converted into a semantic vector using OpenAI embeddings.
- Vector Storage: Embeddings are stored in FAISS for fast similarity search.
- Query Handling: User question is embedded and matched against FAISS to retrieve top-k relevant chunks.
- Answer Generation: Retrieved chunks are passed to GPT-4o-mini to produce a grounded answer.
- Confidence Scoring: Measures relevance of retrieved chunks to indicate answer reliability.

### Confidence Scoring Logic
- Computed based on similarity scores of top-k retrieved chunks
- Higher similarity → higher confidence
Ensures transparency and shows how much the answer is supported by the document

### Project Structure
mini-rag-assistant/
├── app.py                  # Main Streamlit app
├── prompts.py              # System prompt and message builder
├── .env                    # Environment variables (API keys) – NOT in git!
├── requirements.txt        # Dependencies
├── knowledge_base/         # Local uploaded documents (auto-created)
├── knowledge_base_vectorstore/  # FAISS vector store (auto-created)
└── docs/                   # Documentation (optional)

### Setup & Installation

#### Prerequisites
- Python 3.9+
- OpenAI API key[](https://platform.openai.com/account/api-keys)
- (Optional) Azure Storage Account + Blob container

#### Steps

1. **Clone or download** the project

2. **Install dependencies**

   ```bash
   pip install -r requirements.txt

   # .env or RAG.env
OPENAI_API_KEY=sk-proj-XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX

### Optional: Azure Blob Storage
AZURE_STORAGE_CONNECTION_STRING=your-connection string
AZURE_CONTAINER_NAME=your-container-name

### Run the app
streamlit run app.py

### Usage
Choose Local or Cloud (Azure Blob Storage) in the sidebar
For Local:
Upload documents via the uploader
For Cloud:
Ensure .env has valid Azure credentials
App loads documents from your Azure container

Ask questions in the text input box
View answer, metrics, and retrieved chunks

### Troubleshooting
"OPENAI_API_KEY not found"  ----  Check .env  exists and has correct key
401 Invalid API Key -----  Generate a new key from https://platform.openai.com/account/api-keys
No documents loaded --- Upload files (Local) or check Azure container (Cloud)
Azure load fails ----  Verify connection string and container name

### Dependencies
See requirements.txt for full list.
