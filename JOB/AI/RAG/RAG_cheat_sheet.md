# Retrieval-Augmented Generation (RAG) Cheat Sheet

## What is RAG?

Retrieval-Augmented Generation (RAG) is an AI framework that improves the quality of Large Language Model (LLM) responses by retrieving relevant information from an external knowledge base and providing it to the LLM as context.

## Why is RAG Important?

RAG addresses several key limitations of LLMs:

*   **Knowledge Cutoff:** LLMs have no knowledge of events that have occurred since their training data was collected.
*   **Hallucinations:** LLMs can generate plausible but incorrect or nonsensical information.
*   **Lack of Specificity:** LLMs may provide generic answers when domain-specific knowledge is required.
*   **No Source Attribution:** LLMs cannot cite their sources.

## How RAG Works (Basic Workflow)

1.  **Indexing:** External data is processed, chunked, and converted into numerical representations (embeddings) which are stored in a vector database.
2.  **Query:** A user submits a query.
3.  **Retrieval:** The system retrieves the most relevant document chunks from the vector database based on the user's query.
4.  **Augmentation:** The retrieved information is added to the original query to create an "augmented prompt".
5.  **Generation:** The augmented prompt is fed to the LLM, which generates a response based on both its internal knowledge and the provided external context.

## Core Components of a RAG System

*   **Knowledge Base:** The external data source (e.g., documents, databases, APIs).
*   **Retriever:**
    *   **Embedding Model:** Converts text to vectors.
    *   **Vector Database:** Stores and searches embeddings.
*   **Generator:** The LLM that generates the final response.

## Benefits of RAG

*   **Improved Factual Accuracy:** Reduces hallucinations by grounding responses in external data.
*   **Up-to-Date Information:** Allows LLMs to access real-time information without retraining.
*   **Increased Trust and Transparency:** Can provide source attribution.
*   **Cost-Effective:** More economical than constantly retraining LLMs.
*   **Domain-Specific Customization:** Enables LLMs to provide context-aware answers for specific domains.
