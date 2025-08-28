## 🧠 Vector Search
- Helps to find semantically similar items (like documents, images, or queries) by comparing **embedding vectors**.
---

### 🔹 1. Input Stage
- **Input Document**: This could be a document, sentence, or query.
- **Query Text**: Example: "cloud threat"

---

### 🔹 2. Tokenizer
- Splits the input into tokens (subwords or words)
- Maps each token to a **token ID** using a vocabulary
- Example: "cloud threat" → [1234, 5678]

---

### 🔹 3. Embedding Model
- Converts token IDs into **dense vectors**
- Capture the **context** and **semantic meaning** with transformer layer
- Outputs either:
  - Per-token embeddings
  - A single pooled embedding (e.g., fixed-size vector with 1024-dimensional)

---

### 🔹 4. Vector Database
- Stores all document embeddings.

### 🔹 5. Search Query
- New input is tokenized and embedded
- Compared to stored vectors using metrics like **cosine similarity**.
- Returns the **Top Matching Results** based on semantic closeness.

---

## 🌐 Mermaid Diagram

```mermaid
flowchart TD
    subgraph Input["🔹 Input Stage"]
        A[📝 Document]
        G[🔍 Query Text]
    end

    subgraph Tokenization["🔹 Tokenization"]
        B[🧩 Tokenizer]
        H[🧩 Tokenizer]
        C[🔢 Token IDs]
        I[🔢 Query Token IDs]
    end

    subgraph Embedding["🔹 Embedding Model"]
        D[📦 Embedding Model]
        J[📦 Embedding Model]
        E[📈 Embedding Vector]
        K[📈 Query Embedding]
    end

    subgraph Search["🔹 Vector Search"]
        F[🗃️ Vector Database]
        L[📊 Similarity Search]
        M[📄 Top Matching Results]
    end

    A --> B --> C --> D --> E --> F
    G --> H --> I --> J --> K --> L
    F --> L --> M
