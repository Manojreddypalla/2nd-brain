# Model Context Protocol (MCP) Cheat Sheet

## 1. What is Model Context Protocol (MCP)?

MCP is an open standard that allows Large Language Models (LLMs) and AI agents to interact with the "outside world". It provides a standardized way for LLMs to:

*   **Access External Data:** Connect to real-time data, APIs, local files, and databases.
*   **Perform Actions:** Use external tools to send emails, query databases, etc.
*   **Standardize Communication:** Acts as a universal interface for LLMs to discover and use tools.

*Introduced by Anthropic.*

## 2. Why is MCP Important?

*   **Extends LLM Capabilities:** Moves LLMs beyond their training data and internal context window.
*   **Dynamic and Accurate:** Makes LLMs more dynamic and accurate by allowing them to access real-time information.
*   **Enables Complex Workflows:** Allows LLMs to perform multi-step tasks that require interaction with external systems.

## 3. How does MCP work?

MCP provides a standardized way for an LLM to:

1.  **Discover Tools:** The LLM can see what tools are available.
2.  **Understand Tool Usage:** The LLM can understand the inputs and outputs of each tool.
3.  **Invoke Tools:** The LLM can call the tools with structured requests (e.g., JSON).

## 4. Related Concept: Context Window

The **context window** (or context length) is the maximum amount of text (in tokens) that an LLM can process at one time. It's the LLM's "working memory".

*   **Larger Context Window:** Allows for more detailed input and more coherent output.
*   **Challenges:**
    *   **Computational Cost:** Larger context windows require more computational power.
    *   **"Lost in the Middle":** Performance can degrade as the context window gets larger, as the model can struggle to find relevant information.

## 5. Context Management Techniques

To overcome the limitations of context windows, various techniques are used:

*   **Truncation:** Cutting off excess tokens.
*   **Summarization:** Periodically summarizing the context.
*   **Retrieval-Augmented Generation (RAG):** Retrieving only the most relevant information from an external knowledge base.
*   **Memory Buffering:** Storing past conversations to maintain context.
*   **Sliding Window:** Processing text in overlapping segments.
