"# AI-Agent" 

This project implements a Multi-Source Retrieval-Augmented Generation (RAG) system to provide accurate, authority-aware technical support for a fictional software product called NimbusNote Pro.

The system intelligently retrieves information from three different knowledge sources with varying authority levels:

Product Documentation (High Authority)

Technical Blogs (Medium Authority)

Customer Forums (Low Authority)

A Retrieve → Rerank → Generate pipeline is used along with authority-based weighting and LLM-driven contradiction handling to ensure reliable answers.

**Key Features**

Multi-source knowledge ingestion
Authority-weighted vector retrival
Cross-encoder reranking for accuracy
LLM-based contradiction detection and resolution
Query tracking and logging
Evidence-based final answers
Source prioritization (Docs > Blogs > Forums)
Data Sources and Authority Levels
Source Type	File	Description	Authority Weight
Documentation	docs.md	API reference, getting started, sync & backup	1.2 (Highest)
Technical Blogs	blogs.txt	Team workflows, integrations, optimization	1.0 (Medium)
Customer Forums	forums.json	User issues, limits, offline mode, sync errors	0.8 (Lowest)

Each source is chunked differently based on structure and content type.

**Chunking Strategy**

Documentation: Split by sections (each rule in one chunk)
Blogs: Split by complete tutorial sections
Forums: Split into full Q&A pairs for problem-resolution integrity
This ensures semantic coherence during retrieval.

**Retrieval and Combination**

All sources are combined and vectorized into a single embedding matrix
Cosine similarity scores are multiplied by authority weights
weighted_score = similarity_score × source_weight
This ensures that official documentation always dominates when conflicts exist.

**Reranking Mechanism**

A Cross-Encoder model reranks the top retrieved chunks
Query–chunk pairs are rescored for true semantic relevance
Improves top-3 answer accuracy by approximately 20%

**Contradiction Handling**

Uses a local LLM (via LM Studio) for structured contradiction analysis.

The system always:
Evaluates Top-5 chunks
Prioritizes higher authority sources
Produces a structured JSON output:
{
  "status": "conflict_resolved",
  "explanation": "...",
  "authoritative_answer": "..."
}
This prevents hallucinations and conflicting answers.

**Logging and Tracking**

All system activity is logged in:
rag_logs.jsonl
Includes:
User Query
Retrieved Chunks
Reranked Results
Contradiction Analysis

**Final Answer Source**

This enables full traceability and debugging.
Performance Summary
Indexing Time: Few seconds for small datasets
Retrieval Accuracy: High for documentation
Reranking Boost: Approximately 20% relevance improvement
Conflict Detection: Accurate across blogs and forums

**Project Structure**
RAG/
│
├── data/
│   ├── docs.md
│   ├── blogs.md
│   └── forums.txt
│
├── Src/
├── test_index.py
├── chunking.py
├── log_utils.py
├── contradiction_detection.py
├── indexing.py
└── README.md
|_rag_logs.jsonl
pip install -r requirements.txt
python main.py

