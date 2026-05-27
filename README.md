# Glass Box AI: PCATT Career Wayfinder
**Purpose**: A proof-of-concept GraphRAG application designed to provide traceable, reliable career pathways by anchoring local educational data (PCATT) to national cybersecurity standards (NIST NICE) and industry credentials (C3 Mapping).

## Frontend: 
[FE Github Repo](https://github.com/sadieflick/PCATT_KG_Demo_FE.git)
## Backend: 
[BE Github Repo](https://github.com/sadieflick/glassbox_backend_PCATT_demo)
## Data:
[Data Ingestion Reference Repo](https://github.com/sadieflick/NIST_NICE_C3_auraDB_loading)

## The "Fixed Entity" Architecture (FEA)
Unlike traditional RAG which "hallucinates" relationships, this project uses a 3-layer architecture:
1. **The Spine (Layer 1)**: Immutable NIST NICE Work Roles and Competency Areas.
2. **The Evidence (Layer 2)**: Unstructured PCATT course metadata and learner outcomes.
3. **The Bridge (Layer 3)**: Deterministic (NLP) and Probabilistic (Vector) links connecting courses to the spine.

## Key Relationships
- `(:Course)-[:PREPARES_FOR]->(:Certification)`
- `(:Course)-[:ALIGNS_TO]->(:WorkRole)`
- `(:Certification)-[:VALIDATES]->(:WorkRole)`
- `(:WorkRole)-[:RELIANT_ON]->(:CompetencyArea)`

## Tech Stack
- **Database**: Neo4j AuraDB (Graph + Vector)
- **Orchestration**: Python (FastAPI), LangChain/LangGraph
- **Frontend**: React (Vite) + Tailwind


# Implementation Process Explained: Knowledge Graph Ingestion

## Phase 1: Data Normalization
- **Source**: NIST NICE JSON + C3 Mapping CSV.
- **Process**: Extracted `WorkRole` and `CompetencyArea` as unique nodes. 
- **Integrity Check**: Applied `UNIQUE` constraints in Neo4j to prevent entity duplication (De-normalization).

## Phase 2: The "Spine" Construction
- **Mechanic**: Loaded C3 certifications as `(:Certification)` nodes.
- **Logic**: Created `[:VALIDATES]` relationships where C3 identifies a certification as a requirement for a NICE Work Role.

## Phase 3: Unstructured Data Enrichment
- **Tooling**: spaCy (NER) + SentenceTransformers (Embeddings).
- **Process**: 
  1. Splitting PCATT `metadata` into granular `(:CourseOutcome)` nodes.
  2. Generating 384-dimension vectors for each outcome.
  3. Running a "Second Pass" LLM Audit to verify that a "mention" of a certification in a syllabus is actually a "preparatory" goal, not just a passing reference.

## Phase 4: Retrieval Logic
- **Method**: Hybrid Search. 
- **Deterministic**: LLM-to-Cypher for exact Work Role lookups.
- **Probabilistic**: Vector search for "fuzzy" skill gap analysis.