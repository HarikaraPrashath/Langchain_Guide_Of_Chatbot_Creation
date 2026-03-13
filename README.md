# LangChain Chatbot Creation — Complete Study Guide

This repository is a hands-on guide to building a **Retrieval-Augmented Generation (RAG) chatbot** using LangChain, Groq LLM, HuggingFace embeddings, and Chroma vector store. Follow the numbered steps below **in order** to build your understanding from first principles to a fully working chatbot.

---

## Overall RAG Pipeline

```
Document Loading → Splitting → Embedding → Vectorstore → Retrieval → LLM Generation → Response
```

Every notebook in this repo maps to one stage of that pipeline.

---

## Prerequisites

Before starting, make sure you have the following installed:

```bash
pip install langchain langchain-groq langchain-chroma langchain-huggingface langchain-community
pip install pypdf docx2txt python-dotenv numpy
```

Create a `.env` file in the project root with your API key:

```
GROQ_API_KEY=your_groq_api_key_here
```

Get a free Groq API key at [https://console.groq.com](https://console.groq.com).

---

## Step-by-Step Study Order

### ✅ Step 1 — Introduction to LangChain & LLM Integration
📓 **Notebook:** `langChainChatBot.ipynb`

**What you will learn:**
- How to connect to a Large Language Model (LLM) using the Groq API
- How to create a `ChatPromptTemplate` to structure prompts
- How to parse structured (JSON) output from the LLM
- The basics of a LangChain **chain**: `prompt | llm | parser`

**Key LangChain components:** `ChatGroq`, `ChatPromptTemplate`, `JsonOutputParser`

**Why start here?** This notebook introduces the core concept of sending a prompt to an LLM and receiving a structured response — the foundation of every chatbot.

---

### ✅ Step 2 — Loading PDF Documents
📓 **Notebook:** `LoadChunk.ipynb`

**What you will learn:**
- How to load a PDF file using `PyPDFLoader`
- How LangChain represents a document (page content + metadata)
- How to clean and normalize text (strip extra whitespace)
- How to inspect individual pages and their metadata

**Key LangChain components:** `PyPDFLoader`, `Document`

**Why this step?** A chatbot needs data to answer questions from. This notebook shows you how to bring raw PDF files into LangChain as structured `Document` objects.

---

### ✅ Step 3 — Loading Word (DOCX) Documents
📓 **Notebook:** `Document Loading with DOCX2TXT Loader.ipynb`

**What you will learn:**
- How to load Microsoft Word `.docx` files using `Docx2txtLoader`
- How the raw text is extracted and wrapped in a `Document` object
- Differences between PDF and DOCX loading

**Key LangChain components:** `Docx2txtLoader`, `Document`

**Why this step?** Real-world knowledge bases often contain Word documents. This step extends document loading beyond PDFs.

---

### ✅ Step 4 — Splitting Documents by Character
📓 **Notebook:** `Document_Splitting.ipynb`

**What you will learn:**
- Why documents must be split into smaller **chunks** before embedding
- How to configure `chunk_size` (e.g., 500 characters) and `chunk_overlap` (e.g., 50 characters)
- How overlap preserves context across chunk boundaries
- How to use period (`.`) as a separator to split at sentence boundaries

**Key LangChain components:** `CharacterTextSplitter`

**Why this step?** LLMs have context-window limits. Splitting documents into overlapping chunks ensures no information is lost and retrieval stays precise.

---

### ✅ Step 5 — Splitting Documents by Markdown Headers
📓 **Notebook:** `Markdown_Header_Split.ipynb`

**What you will learn:**
- How to split documents based on their **heading structure** (`#`, `##`) for semantic chunking
- How metadata (e.g., chapter name, lecture title) is automatically attached to each chunk
- The difference between character-based splitting and structure-aware splitting

**Key LangChain components:** `MarkdownHeaderTextSplitter`

**Why this step?** Structure-aware splitting produces higher-quality chunks that preserve the meaning of each section, leading to better retrieval accuracy.

---

### ✅ Step 6 — Text Embeddings with Open-Source Models
📓 **Notebook:** `Text_Embedding_with_OpenSource.ipynb`

**What you will learn:**
- What a text **embedding** is (a fixed-size vector that represents meaning)
- How to generate embeddings using the free `all-MiniLM-L6-v2` HuggingFace model
- How to compute **cosine similarity** (dot product) between two embeddings manually
- Why similar sentences produce vectors with a high dot-product score

**Key LangChain components:** `HuggingFaceEmbeddings`

**Why this step?** Embeddings are the backbone of semantic search. Understanding them helps you reason about why certain documents are retrieved for a given query.

---

### ✅ Step 7 — Building a Chroma Vector Store (End-to-End Pipeline)
📓 **Notebook:** `Choma_Vectorstore.ipynb`

**What you will learn:**
- The complete pipeline: **load → split → embed → store**
- How to create a persistent Chroma vector store from a DOCX file
- How to reload a saved vector store from disk without re-embedding
- How `MarkdownHeaderTextSplitter` and `CharacterTextSplitter` are chained together

**Key LangChain components:** `Docx2txtLoader`, `MarkdownHeaderTextSplitter`, `CharacterTextSplitter`, `HuggingFaceEmbeddings`, `Chroma`

**Why this step?** This notebook combines Steps 3–6 into a single, reusable workflow — the foundation of your RAG knowledge base.

---

### ✅ Step 8 — Vector Store CRUD Operations
📓 **Notebook:** `Document_in_VectoreStore.ipynb`

**What you will learn:**
- How to **add**, **update**, and **delete** documents in a Chroma vector store
- How to assign custom IDs and metadata to documents
- How to inspect stored embeddings and their associated text
- How to manage a persistent vector store over time

**Key LangChain components:** `Chroma`, `HuggingFaceEmbeddings`, `Document`

**Why this step?** Real applications need to update their knowledge base. This step teaches you how to maintain and evolve the vector store without rebuilding it from scratch.

---

### ✅ Step 9 — Similarity Search
📓 **Notebook:** `Similarity Search.ipynb`

**What you will learn:**
- How to query a vector store with a natural language question
- How **cosine similarity** is used to rank and return the most relevant document chunks
- How to interpret the retrieved `Document` objects and their metadata

**Key LangChain components:** `Chroma`, `HuggingFaceEmbeddings`

**Why this step?** This is the first half of RAG — retrieving relevant context from the knowledge base before passing it to the LLM.

---

### ✅ Step 10 — Maximal Marginal Relevance (MMR) Search
📓 **Notebook:** `Maximal Marginal Relevance Searc.ipynb`

**What you will learn:**
- The problem of **redundant** results in standard similarity search
- How **MMR** balances relevance and diversity in retrieved results
- How to tune the `lambda_mult` parameter (0 = max diversity, 1 = max relevance)
- When to prefer MMR over plain similarity search

**Key LangChain components:** `Chroma.as_retriever(search_type='mmr')`

**Why this step?** MMR retrieval produces more informative context for the LLM by ensuring the retrieved chunks cover different aspects of the topic.

---

### ✅ Step 11 — Vectorstore-Backed Retriever
📓 **Notebook:** `Vectorstore-Backed Retriever.ipynb`

**What you will learn:**
- How to wrap a vector store in LangChain's standard **Retriever** interface
- How to invoke a retriever with `.invoke("your question")`
- How the retriever interface enables easy plug-in to any LangChain chain

**Key LangChain components:** `vectorstore.as_retriever()`, `retriever.invoke()`

**Why this step?** The retriever abstraction is what allows the vector store to be seamlessly combined with an LLM in the next steps.

---

### ✅ Step 12 — Stuffing Documents into a Prompt
📓 **Notebook:** `Stuffing Documents.ipynb`

**What you will learn:**
- The **Stuffing** strategy: insert all retrieved chunks into one prompt as context
- How to build a RAG chain: `retriever → prompt → LLM → output parser`
- How to use `RunnableParallel` and `RunnablePassthrough` to pass both context and the question to the prompt
- The context-window limits of this approach

**Key LangChain components:** `Chroma`, `PromptTemplate`, `RunnablePassthrough`, `RunnableParallel`, `ChatGroq`, `StrOutputParser`

**Why this step?** This is the first complete RAG chain. The LLM now answers questions grounded in your own documents.

---

### ✅ Step 13 — Generating a Final Response (Complete RAG Chatbot)
📓 **Notebook:** `Generating a Response.ipynb`

**What you will learn:**
- The complete, production-ready **RAG pipeline** from query to answer
- How to load a persisted vector store and wire it to a Groq LLM
- How to format a prompt with `{context}` and `{question}` placeholders
- How to extract clean string answers with `StrOutputParser`

**Key LangChain components:** `Chroma`, `HuggingFaceEmbeddings`, `ChatGroq`, `PromptTemplate`, `RunnablePassthrough`, `RunnableParallel`, `StrOutputParser`

**Why end here?** This notebook ties together every previous step into a single working chatbot that retrieves relevant knowledge and generates accurate, grounded answers.

---

## Quick Reference: Study Order Summary

| Step | Notebook | Topic |
|------|----------|-------|
| 1 | `langChainChatBot.ipynb` | LLM basics & prompt chains |
| 2 | `LoadChunk.ipynb` | Loading PDF documents |
| 3 | `Document Loading with DOCX2TXT Loader.ipynb` | Loading DOCX documents |
| 4 | `Document_Splitting.ipynb` | Character-based text splitting |
| 5 | `Markdown_Header_Split.ipynb` | Header-based semantic splitting |
| 6 | `Text_Embedding_with_OpenSource.ipynb` | Text embeddings & similarity |
| 7 | `Choma_Vectorstore.ipynb` | Building a Chroma vector store |
| 8 | `Document_in_VectoreStore.ipynb` | Vector store CRUD operations |
| 9 | `Similarity Search.ipynb` | Semantic similarity search |
| 10 | `Maximal Marginal Relevance Searc.ipynb` | MMR diversity-aware search |
| 11 | `Vectorstore-Backed Retriever.ipynb` | Retriever interface |
| 12 | `Stuffing Documents.ipynb` | Stuffing strategy RAG chain |
| 13 | `Generating a Response.ipynb` | Complete RAG chatbot |

---

## Project Tech Stack

| Component | Technology |
|-----------|-----------|
| LLM | [Groq](https://console.groq.com) — `llama-3.3-70b-versatile` |
| Embeddings | [HuggingFace](https://huggingface.co) — `all-MiniLM-L6-v2` |
| Vector Store | [Chroma](https://www.trychroma.com) |
| Orchestration | [LangChain](https://python.langchain.com) |
| Language | Python 3.10+ |

---

> **Note on filenames:** Some notebook filenames contain minor typos from the original repository (`Choma_Vectorstore.ipynb` instead of `Chroma_Vectorstore.ipynb`, `Document_in_VectoreStore.ipynb`, `Maximal Marginal Relevance Searc.ipynb`). The filenames in this guide match the actual files on disk exactly.

---

## Tips for Learners

- **Run each notebook top to bottom** — cells depend on earlier cells.
- **Steps 7–13 depend on each other** — the vector store built in Step 7 is reused from Step 9 onwards.
- **The `.env` file** must be in the project root for API key loading to work.
- **First embedding run** (Step 6 and 7) downloads the HuggingFace model (~90 MB) — this only happens once.
- If you see a `persist_directory` in a notebook, make sure that path exists on your machine.
