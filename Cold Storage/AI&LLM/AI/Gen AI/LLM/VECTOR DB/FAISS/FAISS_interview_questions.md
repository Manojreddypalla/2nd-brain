# FAISS Interview Questions

### 1. What is FAISS?
**Answer:** FAISS (Facebook AI Similarity Search) is an open-source library for efficient similarity search and clustering of dense vectors, designed for large-scale applications.

### 2. Why is FAISS used?
**Answer:** It solves the problem of finding similar items in massive, high-dimensional datasets quickly, which is crucial for applications like recommendation systems and search engines.

### 3. What's the difference between exact and approximate nearest neighbor (ANN) search in FAISS?
**Answer:**
*   **Exact Search (`IndexFlatL2`):** Compares the query vector with every other vector to find the absolute closest neighbors. It's accurate but slow for large datasets.
*   **ANN Search (`IndexIVF`, `HNSW`):** Finds "close enough" neighbors without guaranteeing the absolute closest ones. It's much faster and suitable for real-time systems.

### 4. What are some common indexing approaches in FAISS?
**Answer:**
*   **`IndexFlatL2`:** Brute-force search.
*   **`IndexIVF`:** Partitions the data into clusters (Voronoi cells) to reduce the search space.
*   **Product Quantization (PQ):** Compresses vectors to reduce memory usage.
*   **HNSW (Hierarchical Navigable Small World):** A graph-based approach for fast and accurate ANN search.

### 5. What does it mean to "train" a FAISS index?
**Answer:** Training involves learning the data's distribution to optimize the index structure. Indexes like `IndexIVF` require training to learn cluster centroids, while `IndexFlatL2` does not.

### 6. Can you use an index trained on one dataset for another?
**Answer:** No. The index is optimized for the specific data distribution it was trained on. Using it on a different dataset will likely result in poor performance.

### 7. What are some real-world applications of FAISS?
**Answer:**
*   Recommendation Systems
*   Semantic Search Engines
*   Anomaly Detection
*   Clustering
*   Genomics

### 8. What are some challenges when working with FAISS?
**Answer:**
*   **Memory Management:** Large embeddings can be memory-intensive.
*   **Accuracy vs. Speed Trade-off:** Choosing the right index involves balancing these factors.
*   **Training Data:** The quality of training data is crucial for some indexes.
*   **Updating/Deleting Vectors:** FAISS is optimized for static datasets, so updates can be complex.

### 9. When would you choose FAISS over a vector database like Milvus or Pinecone?
**Answer:** FAISS is a library that gives you fine-grained control over indexing algorithms. You would choose it for high-performance, custom solutions where you manage the infrastructure. Vector databases like Milvus or Pinecone are managed services that abstract away the complexities of index management and are often easier to integrate into applications.
