# Glass Box AI: PCATT Career Wayfinder
**Purpose**: A proof-of-concept GraphRAG application designed to provide traceable, reliable career pathways by anchoring local educational data (PCATT) to national cybersecurity standards (NIST NICE) and industry credentials (C3 Mapping).

## Frontend: [FE Github Repo] ()
## Backend: [BE Github Repo] ()

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