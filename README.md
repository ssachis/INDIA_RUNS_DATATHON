# INDIA_RUNS_DATATHON
Initial dataset size=100k 

100,000 Candidates

        │
        ▼
        
Candidate Pool Filtering
(Deterministic)

        │
        ▼
        
~4,000 Candidates
        │
        ▼
        
Final Filtering
(Deterministic scoring)

        │
        ▼
        
~150 Candidates

        │
        ▼
        
OpenAI LLM Ranking
(using structured JD)

        │
        ▼

Top 100 Submission



# AI Candidate Ranking Pipeline

A scalable hybrid candidate ranking system that identifies the **Top 100 candidates** from a dataset of **100,000+ profiles** using deterministic filtering followed by **LLM-based semantic ranking**.

Instead of evaluating every candidate with an LLM (which is expensive and slow), this pipeline progressively narrows the search space using rule-based filtering and feature engineering before performing a final semantic evaluation using OpenAI.

---

## Pipeline Overview

```
100,000 Candidate Profiles
          │
          ▼
Candidate Pool Filtering
(Data Cleaning + Timeline Validation + AI Skill Retrieval)
          │
          ▼
~4,000 Candidates
          │
          ▼
Final Filtering
(Experience + Behaviour + Skill Matching)
          │
          ▼
~150 Candidates
          │
          ▼
OpenAI Semantic Ranking
(Structured Job Description + Candidate Profiles)
          │
          ▼
Top 100 Ranked Candidates
```

---

# Repository Structure

```
.
├── Candidate Pool Filtering.ipynb
├── Final_Filtering.ipynb
├── IRA_VORA.ipynb
├── openai.ipynb
├── candidate_pool.json
├── jd_structured.json
├── llm_candidates.pkl
├── top_100_candidates.csv
└── README.md
```

---

# Project Workflow

## Stage 1 – Candidate Pool Filtering

**Input:** ~100,000 candidate profiles

This notebook performs the initial filtering to eliminate irrelevant or low-quality profiles while preserving technically relevant candidates.

### Data Cleaning

Removes candidates with:

- Invalid salary information
- Incomplete profiles
- Suspicious recruiter behaviour
- Invalid interview records
- Missing critical information

### Timeline Validation

Validates:

- Education timelines
- Employment timelines
- Career progression consistency
- Impossible date overlaps

### Non-Target Profile Removal

Filters out candidates clearly unrelated to technical AI roles, including:

- Recruiters
- HR
- Sales
- Marketing
- Finance
- Customer Support
- Graphic Designers

### AI Skill Retrieval

Candidate profiles are searched for AI and Information Retrieval related skills such as:

- Machine Learning
- Deep Learning
- NLP
- LLMs
- RAG
- Recommendation Systems
- Semantic Search
- Vector Search
- Elasticsearch
- FAISS
- Pinecone
- TensorFlow
- PyTorch
- Python

Candidates receive a retrieval score based on skill relevance.

### Output

```
candidate_pool.json
```

Dataset size:

```
100,000
    ↓
~4,000 Candidates
```

---

# Stage 2 – Job Description Structuring

The raw Job Description is processed using an LLM to extract structured hiring requirements.

The generated JSON includes:

- Required skills
- Preferred skills
- Experience requirements
- Technical priorities
- Domain expertise
- Hidden hiring intent

### Output

```
jd_structured.json
```

This structured representation is later used during candidate ranking.

---

# Stage 3 – Final Filtering

**Input:** ~4,000 candidates

This stage computes deterministic scores before LLM evaluation.

### Experience Filtering

Candidates are filtered according to required years of experience.

(Current implementation: **4–10 years**)

### Behaviour Scoring

Uses recruiter and engagement signals such as:

- Recruiter response rate
- Interview completion rate
- Notice period
- Relocation willingness
- Last active date
- Profile completeness
- GitHub activity

Produces:

```
behavior_score
```

### Skill Scoring

Each technical skill is scored using:

- Proficiency
- Experience
- Endorsements

### Job Description Skill Matching

Candidates are evaluated against Information Retrieval and AI skills extracted from the structured job description.

Examples include:

- Information Retrieval
- Learning to Rank
- Recommendation Systems
- Vector Search
- Elasticsearch
- OpenSearch
- Pinecone
- FAISS
- Milvus
- Weaviate
- BM25
- NLP
- Python

Produces:

```
jd_skill_score
```

### Deterministic Candidate Ranking

Candidates are ranked using a weighted score:

```
Pre-LLM Score =
0.75 × JD Skill Score
+ 0.15 × Behaviour Score
+ 0.10 × IR Match Count
```

### Candidate Profile Generation

For every shortlisted candidate, a compact profile is generated containing:

- Professional Summary
- Current Role
- Experience
- Career History
- Skills
- Certifications

### Output

```
llm_candidates.pkl
```

Dataset size:

```
4,000
    ↓
~150 Candidates
```

---

# Stage 4 – OpenAI Semantic Ranking

**Input**

- `llm_candidates.pkl`
- `jd_structured.json`

The final stage uses an OpenAI model to rank the remaining candidates based on semantic understanding rather than simple keyword matching.

The model evaluates:

- Technical expertise
- Career progression
- Overall relevance
- Behavioural signals
- Hiring fit
- Quality of experience

The resulting ranked list forms the final submission.

### Output

```
top_100_candidates.csv
```

---

# IRA_VORA.ipynb

This notebook was developed as a prototype to validate the complete evaluation pipeline on a single candidate before scaling to the full dataset.

It demonstrates:

- Candidate parsing
- Career history extraction
- Behaviour scoring
- Skill scoring
- Profile generation
- LLM evaluation

---

# Why a Hybrid Pipeline?

Running an LLM on 100,000 candidates is computationally expensive and unnecessary.

Instead, the pipeline follows a staged approach:

1. **Deterministic filtering** removes obviously irrelevant candidates.
2. **Feature engineering** scores technical and behavioural attributes.
3. **LLM reasoning** is reserved only for the strongest candidates.

This significantly reduces inference cost while preserving ranking quality.

---

# Dataset Reduction

| Stage | Candidates Remaining |
|---------|---------------------:|
| Initial Dataset | 100,000+ |
| Candidate Pool Filtering | ~4,000 |
| Final Filtering | ~150 |
| OpenAI Ranking | Top 100 |

---

# Outputs

| File | Description |
|------|-------------|
| `candidate_pool.json` | Candidate pool after initial filtering |
| `jd_structured.json` | Structured representation of the Job Description |
| `llm_candidates.pkl` | Final shortlist before LLM ranking |
| `top_100_candidates.csv` | Final ranked Top 100 candidates |

---

# Tech Stack

- Python
- Pandas
- NumPy
- OpenAI API
- Google Gemini (JD Structuring)
- JSON
- Pickle

---

# Summary

This project implements a scalable hybrid recruitment pipeline capable of ranking candidates from a dataset of over **100,000 profiles**. By combining deterministic filtering with semantic LLM reasoning, the system efficiently narrows the candidate pool while maintaining high-quality rankings. The approach balances speed, reproducibility, and cost efficiency, making it suitable for large-scale AI-assisted hiring workflows.
