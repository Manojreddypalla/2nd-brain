# FAISS Cheat Sheet

## What is FAISS?

FAISS (Facebook AI Similarity Search) is a library for efficient similarity search and clustering of dense vectors. It's designed for large datasets and supports GPU acceleration.

### 1. Installation

*   **CPU:** `pip install faiss-cpu` or `conda install -c pytorch faiss-cpu`
*   **GPU:** `pip install faiss-gpu` or `conda install -c pytorch faiss-gpu`

### 2. Core Concepts

*   **Vectors:** FAISS works with collections of fixed-size vectors (embeddings).
*   **Index:** A data structure that stores vectors for efficient searching.
*   **Distance Metrics:**
    *   **Euclidean Distance (L2):** Straight-line distance.
    *   **Inner Product (IP):** For cosine similarity with normalized vectors.
    *   **Cosine Similarity:** Angle between vectors.

### 3. Basic Usage: `IndexFlatL2` (Brute-Force)

'''python
import faiss
import numpy as np

d = 64  # vector dimension
nb = 10000  # database size
xb = np.random.random((nb, d)).astype('float32')

index = faiss.IndexFlatL2(d)
index.add(xb)

nq = 5  # number of queries
xq = np.random.random((nq, d)).astype('float32')
k = 4  # find 4 nearest neighbors

D, I = index.search(xq, k)
print("Distances:", D)
print("Indices:", I)
'''

### 4. Advanced Indexes

*   **`IndexIVFFlat`:** Uses an inverted file system to partition data into clusters, speeding up search. Requires training.
*   **`IndexIVFPQ`:** Combines IVF with Product Quantization (PQ) to compress vectors and reduce memory. Requires training.
*   **`IndexHNSW`:** Graph-based index for fast and accurate approximate nearest neighbor search.

### 5. Saving and Loading Indexes

'''python
# Save
faiss.write_index(index, "my_index.bin")

# Load
loaded_index = faiss.read_index("my_index.bin")
'''

### 6. GPU Usage

'''python
res = faiss.StandardGpuResources()
gpu_index = faiss.index_cpu_to_gpu(res, 0, index)
gpu_index.add(xb)
D_gpu, I_gpu = gpu_index.search(xq, k)
'''

### 7. Key Benefits

*   **High-Speed Search:** Highly optimized algorithms.
*   **Scalability:** Handles billions of vectors.
*   **GPU Acceleration:** Significant speedup for real-time applications.
*   **Versatility:** Used in image recognition, NLP, recommendations, etc.
*   **Flexibility:** Multiple index types and distance metrics.
