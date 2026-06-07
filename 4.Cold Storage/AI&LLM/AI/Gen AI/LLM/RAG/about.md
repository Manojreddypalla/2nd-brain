**RAG** stands for **Retrieval-Augmented Generation**.

Think of it this way:

- An **LLM** (like ChatGPT or Gemini) is like a very smart student who has read a *massive* library of books but is now locked in a room. They only know what they learned up to the point they entered the room (their "knowledge cutoff").
- **RAG** is like giving that student a laptop with access to a specific, up-to-date database or the internet.

The Problem RAG Solves

Standard LLMs have two major limitations:

1. **Knowledge Cutoff:** Their knowledge is static and stops at the date their training data was collected. They don't know about current events, your private documents, or your company's internal wiki.
2. **Hallucination:** When an LLM doesn't know the answer, it has a tendency to "make up" a confident-sounding but incorrect answer. This is also known as hallucination.

RAG allows a model to use only the most relevant information for each
query, reducing the number of input tokens while potentially increasing the model’s
performance.