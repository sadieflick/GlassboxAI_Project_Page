### **Architectural Comparison: Data Retrieval & Reasoning**

| Feature | **Direct Text-to-SQL** | **Semantic Layer to SQL** | **Knowledge Graph Traversal** |
| --- | --- | --- | --- |
| **Logic** | **Probabilistic:** The LLM "guesses" the query based on table names. | **Deterministic:** The LLM picks from a "menu" of pre-defined metrics/dimensions. | **Relational Reasoning:** The AI "walks" explicit paths between entities. |
| **Storage** | Relational DB (Postgres, MySQL). | Relational DB + YAML/Metadata (dbt, Looker). | Graph DB (Neo4j, Memgraph). |
| **Accuracy** | **60–80%:** High hallucination risk on complex joins. | **95–100%:** Guaranteed correctness if the metric is modeled. | **High (85%+):** Explains *why* two things are connected (Provenance). |
| **Maintenance** | **Low:** Just point the AI at the schema. | **High:** Requires manual modeling of every business metric. | **Medium:** Requires an initial "Spine" (Ontology) but is flexible. |
| **Ideal For** | Ad-hoc questions on clean, simple schemas. | Corporate KPIs, Finance, Board-level Reporting. | **Career Paths, Fraud, Supply Chain, Recommendations.** |

---

### **Deep Dive: Strengths & Drawbacks**

#### **1. Direct Text-to-SQL (The "Quick & Dirty")**

* **Mechanism:** You provide the LLM with the DB schema (DDL) and it writes code.
* **Drawback (The "Broken Join" Problem):** If your table is named `EMP` and another is `SAL`, the LLM might guess how to join them, but it can easily get the "Join Logic" wrong or use the wrong date filter.
* **Determinism:** Zero. The same question might result in two different SQL queries.

#### **2. Semantic Layer to SQL (The "Titanium Layer")**

* **Mechanism:** You define a "Directed Acyclic Graph" (DAG) of your metrics (e.g., `Revenue = Price * Quantity`). The LLM doesn't write SQL; it says *"I need the 'Revenue' metric grouped by 'Region'."* A compiler (like dbt MetricFlow) then writes the "perfect" SQL.
* **Benefit:** **Total Governance.** It is impossible for the AI to "hallucinate" a calculation that hasn't been approved by a human.
* **Drawback:** It cannot answer questions about things you haven't modeled. It’s a "Closed World."

#### **3. Knowledge Graph Traversal (The "Glass Box")**

* **Mechanism:** Instead of searching for "similar" rows (Vector) or "joining" tables (SQL), the system follows a path: `User -> Skill -> Certification -> Course`.
* **The "Fishbone" Benefit:** This resolves **Semantic Ambiguity**. In SQL, "Cybersecurity" and "Information Security" are just two different strings. In a Knowledge Graph, they are both linked to the same **Fixed Entity (NIST Role)**, so the AI knows they are the same thing.
* **Vector Embeddings (Hybrid):** You use embeddings to *discover* which node to start at, and the Graph Traversal to *explain* the path.

---

### **Summary of Benefits for this Use Case**

**GraphRAG (FEA)** the "Third Way":

1. **SQL** is too rigid and doesn't understand "messy" career descriptions.
2. **Vector Search** is too "fuzzy" and might recommend a random course just because it mentions a keyword.
3. **Knowledge Graph** provides the **Audit Trail**. It uses the flexibility of AI to understand the user's intent, but the **determinism** of a graph to prove that the course actually meets the NIST requirement.

### **Why "DAG Modeling" matters in SQL**

In traditional RDBMS, a DAG (Semantic Layer) is used to prevent "Circular Dependencies" and ensure that the AI follows a clear, non-looping logic when calculating data. In this example, it is essentially doing the same thing: **Ensuring the path from Student to Career is a clear, logical progression.**

### Relevant Resources

[GraphRAG vs RAG: Why you need Knowledge Graphs for AI](https://www.youtube.com/watch?v=S67l8xMK6Fs)

[Semantic Layer vs Text-to-SQL](https://docs.getdbt.com/blog/semantic-layer-vs-text-to-sql-2026?version=1.13)