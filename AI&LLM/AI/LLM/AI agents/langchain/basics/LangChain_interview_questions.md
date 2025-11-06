# LangChain Interview Questions

### 1. What is LangChain and its primary goal?
**Answer:** LangChain is a framework for developing AI applications powered by large language models (LLMs). Its main goal is to simplify the creation of complex, context-aware, and multi-step LLM applications by providing modular components and abstractions.

### 2. What are the core components of a LangChain application?
**Answer:**
*   **Models (LLMs, Chat Models, Embedding Models):** The core reasoning engine.
*   **Prompts:** Templates for generating inputs to models.
*   **Chains:** Sequences of calls to models or other components.
*   **Agents and Tools:** Agents use tools to interact with the outside world.
*   **Memory:** Persists state across interactions.
*   **Document Loaders, Text Splitters, Vector Stores, and Retrievers:** The building blocks for Retrieval Augmented Generation (RAG).

### 3. What is the difference between a Chain and an Agent?
**Answer:**
*   **Chains** follow a predetermined sequence of steps. The workflow is hardcoded.
*   **Agents** use an LLM to decide which actions to take. The workflow is dynamic and determined by the agent at runtime.

### 4. Explain Retrieval Augmented Generation (RAG) in the context of LangChain.
**Answer:** RAG is a technique that enhances LLM responses by providing them with external knowledge. In LangChain, this is typically done by:
1.  Loading documents from a source.
2.  Splitting the documents into smaller chunks.
3.  Creating embeddings (numerical representations) of the chunks.
4.  Storing the embeddings in a vector store.
5.  When a query is received, retrieving the most relevant chunks from the vector store.
6.  Passing the retrieved chunks along with the query to the LLM to generate a response.

### 5. What is the role of Memory in LangChain?
**Answer:** Memory allows chains and agents to remember past interactions. This is crucial for building conversational applications like chatbots, where context needs to be maintained across multiple turns of a conversation.

### 6. How do you create a custom tool for a LangChain agent?
**Answer:** You can create a custom tool by defining a Python function and then wrapping it with LangChain's `Tool` class. The `Tool` class takes a name, the function, and a description of what the tool does. The agent then uses the description to decide when to use the tool.

### 7. What is LangChain Expression Language (LCEL)?
**Answer:** LCEL is a declarative way to compose chains and other LangChain components. It uses the `|` (pipe) operator to chain components together, making the code more readable and composable.

### 8. What are some limitations of LangChain?
**Answer:**
*   **Complexity:** Can be more complex than using raw LLM APIs for simple tasks.
*   **Debugging:** Debugging can be challenging, especially with agents that make their own decisions.
*   **Latency:** Multi-step chains can introduce latency.
*   **Security:** Agents that use tools need to be carefully designed to prevent them from being used in unintended or harmful ways.
