## 🧠 **LangChain Cheat Sheet (2025 Quick Review)**

### 🔹 1. **What is LangChain?**

LangChain helps you **build LLM-powered applications** that can:

- Use **external data** (databases, APIs, documents)
- **Reason** with tools (calculators, search)
- Manage **multi-step chains** (question → retrieval → summarization)
- Maintain **memory** across conversations

---

### 🔹 2. **Core Concepts**

| Concept | Description | Example |
| --- | --- | --- |
| **LLM** | Interface to any language model | `from langchain.llms import OpenAI` |
| **PromptTemplate** | Structure input prompts | `PromptTemplate("Translate {text} to French")` |
| **Chain** | Combines components (Prompt → LLM → Output) | `LLMChain(llm=llm, prompt=prompt)` |
| **Tool** | External utility (e.g., calculator, API) | `Tool(name="Calculator", func=math_tool)` |
| **Agent** | LLM that decides which tool to use | `initialize_agent(tools, llm, agent_type="zero-shot-react-description")` |
| **Memory** | Stores past context | `ConversationBufferMemory()` |
| **Retriever** | Fetches relevant chunks from vector DB | `Chroma.from_documents(docs, embeddings)` |

---

### 🔹 3. **Common Workflows**

### 🧩 a. **Simple LLM Chain**

```python
from langchain import OpenAI, PromptTemplate, LLMChain

llm = OpenAI(model="gpt-3.5-turbo")
prompt = PromptTemplate(input_variables=["topic"], template="Explain {topic} simply.")
chain = LLMChain(llm=llm, prompt=prompt)

response = chain.run("quantum computing")
print(response)

```

### 🧩 b. **Document Q&A (Retrieval-Augmented Generation)**

```python
from langchain.chains import RetrievalQA
from langchain.vectorstores import Chroma
from langchain.embeddings import OpenAIEmbeddings
from langchain.llms import OpenAI
from langchain.document_loaders import TextLoader

# Load and index documents
loader = TextLoader("notes.txt")
docs = loader.load()
db = Chroma.from_documents(docs, OpenAIEmbeddings())

qa = RetrievalQA.from_chain_type(llm=OpenAI(), retriever=db.as_retriever())
qa.run("What are LangChain components?")

```

### 🧩 c. **Tool-Using Agent**

```python
from langchain.agents import initialize_agent, load_tools
from langchain.llms import OpenAI

llm = OpenAI(temperature=0)
tools = load_tools(["serpapi", "llm-math"], llm=llm)
agent = initialize_agent(tools, llm, agent_type="zero-shot-react-description")

agent.run("What's 12% of the population of France?")

```

---

### 🔹 4. **Memory Types**

| Memory Type | Purpose |
| --- | --- |
| `ConversationBufferMemory` | Stores entire chat history |
| `ConversationSummaryMemory` | Summarizes past interactions |
| `ConversationBufferWindowMemory` | Keeps last *n* messages |
| `VectorStoreRetrieverMemory` | Semantic memory using embeddings |

---

### 🔹 5. **Vector Stores**

Used for **semantic search** and **retrieval**.

- **Common Stores**: Chroma, FAISS, Pinecone, Milvus
- **Workflow**:
    1. Load docs →
    2. Embed them →
    3. Store in vector DB →
    4. Retrieve with similarity search.

---

### 🔹 6. **Popular Integrations**

- 🧾 **OpenAI** — GPT models
- 🔍 **SerpAPI** — web search
- 📚 **Chroma / Pinecone** — vector DBs
- 🗣 **LangServe** — deploy LangChain apps
- ⚙️ **LangGraph** — visualize chain/agent logic

---

### 🔹 7. **Quick Tips**

✅ Use **`LangSmith`** for debugging & monitoring chains

✅ Prefer **retrievers** over static prompts for large data

✅ Combine **memory + tools + retrieval** for advanced chatbots

✅ For deployment: use **LangServe + FastAPI**

---

### ⚡ **Minimal End-to-End Example**

```python
from langchain import OpenAI, LLMChain, PromptTemplate

prompt = PromptTemplate.from_template("Summarize: {text}")
llm = OpenAI(temperature=0)
chain = LLMChain(prompt=prompt, llm=llm)

print(chain.run("LangChain simplifies building LLM apps."))

```