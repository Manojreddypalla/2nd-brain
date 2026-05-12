# work flow

## ⚙️ **1️⃣ Indexing Workflow (One-time setup per repo)**

> Purpose: Prepare “memory” for the repo → embeddings → FAISS index.
> 

---

### 🩵 **Step 1 — Input**

**Component:** Web UI / n8n webhook / API endpoint

**Action:** User submits repo URL.

**Example:**

`POST /analyze_repo`

Payload: `{ "repo_url": "https://github.com/user/project" }`

---

### ⚙️ **Step 2 — Clone Repository**

**Component:** GitPython

**Action:** Clone the repo locally into a workspace folder.

**Output:** Local repo files on disk.

---

### 📁 **Step 3 — File Parsing**

**Component:** Python script

**Action:** Traverse all directories and filter files:

- Skip `.git/`, binaries, and large media files.
- Collect `.py`, `.js`, `.md`, `.json`, etc.

**Output:** A list of files + their content + metadata.

---

### ✂️ **Step 4 — Chunking**

**Component:** LangChain’s `RecursiveCharacterTextSplitter`

**Action:** Break long files into smaller chunks (e.g. 800–1,000 tokens each).

**Goal:** Make each piece small enough for LLM input later.

**Output:** Chunks of text with metadata (file name, path).

---

### 🧬 **Step 5 — Embedding Generation**

**Component:** OpenAI / Hugging Face Embedding API

**Action:** Convert each text chunk into a **vector embedding**.

**Each vector:** 1,536 floats (≈6 KB per chunk).

**Output:** (chunk_id, vector, metadata)

---

### 💾 **Step 6 — Store in FAISS**

**Component:** FAISS index (HNSW or IVF Flat)

**Action:** Add all vectors to FAISS index.

**Save index to disk:** `repo_name.index`

**Output:** Searchable FAISS vector memory for that repo.

---

### 🗃️ **Step 7 — Store Metadata in MySQL**

**Component:** MySQL

**Action:** Insert record in database:

| Field | Example |
| --- | --- |
| repo_name | "requests" |
| repo_url | "[https://github.com/psf/requests](https://github.com/psf/requests)" |
| index_path | "/data/requests.index" |
| total_files | 124 |
| total_chunks | 960 |
| analyzed_at | timestamp |

**Output:** DB entry to identify the repo.

---

### ✅ **Step 8 — Confirm Completion**

**Component:** API response or n8n node

**Output:**

`{"status": "indexed", "repo_id": 12}`

Now your repo’s “memory” is ready in FAISS.

---

## 💬 **2️⃣ Query Workflow (Whenever user asks a question)**

> Purpose: Retrieve relevant code context → send to LLM → generate explanation.
> 

---

### 💡 **Step 1 — User Question**

**Component:** Web UI / CLI / API

**Example:**

`"How does this repo handle database connections?"`

---

### ⚙️ **Step 2 — Fetch Repo Context**

**Component:** Backend API

**Action:** Identify which repo the question refers to (via `repo_id`).

---

### 🧬 **Step 3 — Embed the Question**

**Component:** Same embedding model (for consistency)

**Action:** Convert user question → vector embedding.

**Output:** 1 question vector.

---

### 🔍 **Step 4 — Semantic Search (FAISS Retrieval)**

**Component:** FAISS

**Action:**

Compare question vector with stored repo vectors.

Find **Top N (e.g., 5)** closest matches.

**Output:** The most relevant chunks of code/docs + metadata (file paths).

---

### 📦 **Step 5 — Context Assembly**

**Component:** Backend logic / LangChain

**Action:** Combine those top chunks into a single context string.

Example:

```
Context:
[Chunk1: auth.py lines 12-60]
[Chunk2: db_connection.py lines 5-40]
Question: "How does this repo handle database connections?"

```

---

### 🤖 **Step 6 — Send to LLM**

**Component:** OpenAI GPT API (ChatCompletion endpoint)

**Action:**

Prompt the LLM with:

```
"You are an AI assistant. Based on the provided code context, answer the question precisely."

<context_here>
<question_here>

```

**Output:** Natural language explanation.

---

### 🧠 **Step 7 — Post-process & Summarize**

**Component:** Backend logic

**Action:**

Optionally clean up LLM output, add citations (file names, line numbers).

**Output:** Final response object:

```json
{
  "answer": "The repo connects to MySQL using SQLAlchemy...",
  "sources": ["db_connection.py", "config.py"]
}

```

---

### 🧭 **Step 8 — Visualization (Optional)**

**Component:** D3.js / Mermaid.js / Graphviz

**Action:**

Generate a mind map showing:

- Repo hierarchy
- Files referenced in the answer
- Relationships (imports, dependencies)

**Output:** Interactive graph.

---

### 💾 **Step 9 — Store Query Log**

**Component:** MySQL

**Action:** Store each Q&A for later analysis (query history, accuracy, usage).

---

### 🩵 **Step 10 — Return Output to User**

**Component:** Web UI / API response / n8n output node

**Output:**

- LLM answer
- Source files
- Mind map link

---

## 🔄 **Combined Data Flow Summary (Both Workflows)**

```
┌──────────────────────────────────────────────┐
│                USER / CLIENT                 │
│  • Input repo URL  • Ask repo questions      │
└──────────────────────────────────────────────┘
                │
                ▼
   ┌──────────────────────────┐
   │        BACKEND API       │
   │(FastAPI / n8n Webhook)   │
   └──────────────────────────┘
                │
        ┌───────────────┐
        │  Indexing     │  <─── Repo URL
        └───────────────┘
           │    │
           ▼    ▼
     GitPython   File Parser
           │
           ▼
     Chunking → Embeddings → FAISS Index → MySQL Metadata
           │
           ▼
     (Repo memory ready)

Query Stage:
User Question → Embedding → FAISS Search → Context → LLM → Response → Visualization → User

```

---

## ⚡ **Data Movement Summary Table**

| Step | From | To | Data Type |
| --- | --- | --- | --- |
| 1 | User | Backend | JSON (repo_url / question) |
| 2 | Backend | GitPython | URL (string) |
| 3 | GitPython | Disk | Repo files |
| 4 | Files | Text Splitter | Plain text |
| 5 | Text | Embedding Model | Vector (float32 array) |
| 6 | Embeddings | FAISS | Vector index |
| 7 | Metadata | MySQL | Structured rows |
| 8 | User Query | Embedding Model | Vector |
| 9 | FAISS | Backend | Top-N chunks |
| 10 | Backend | LLM | Context + question |
| 11 | LLM | Backend | Answer text |
| 12 | Backend | UI | JSON (answer + sources) |

---

## 🧠 **Pro Optimization Ideas**

- Cache embeddings for repeated questions (Redis).
- Store FAISS index path in DB for multi-repo queries.
- Use batch embedding for multiple files at once (to cut cost).
- Compress FAISS index with Product Quantization if it grows large.
- Add auto-suggested questions using LLM (e.g., “What to ask next?”).

---

✅ **In short:**

> RepoMind’s workflow turns repos → embeddings → searchable memory → intelligent answers — all orchestrated by your backend and powered by FAISS + LLM synergy.
>