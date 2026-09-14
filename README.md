# COSC2669 WIL Project — Group 20

## Group ID: 20

### Team Members

| Student ID | Full Name        |
| ---------- | ---------------- |
| s4197741   | Ghislaine Olivar |
| s4036275   | Sara Joshi       |
| s4204467   | Zois Stavrakas   |
| s4191093   | Suzy Doan        |

## Walert Reproduction

We reproduced the RAG pipeline from [Walert](https://github.com/rmit-ir/walert) (RMIT's FAQ chatbot for the School of Technologies) as a starting point for our own project.

**Approach:**

* **Retrieval:** BM25 (via `rank_bm25`), following Walert's lexical retrieval baseline
* **Generation:** Ollama (`llama3.2:3b`), run locally at no cost, using a prompt structure adapted from Walert's `RAG_SYSTEM.py`
* **Validation:** Pipeline tested end-to-end against Walert's own dataset (120 FAQ passages, 96 labelled test questions)

**Preliminary results:**

* 67.71% top-3 retrieval accuracy (65/96 questions), evaluated against Walert's provided relevance judgements (`qrels.txt`)
* Generation produces grounded answers when retrieval succeeds, but is sensitive to question phrasing, where different paraphrasings of the same underlying question can retrieve different passages
* Observed run-to-run variability from local LLM generation, including one case where the system correctly returned "NA" rather than hallucinating an answer

**Notebook:** `Walert_Reproduction.ipynb` — contains the full pipeline, including data loading, BM25 indexing, retrieval evaluation and generation testing.

This reproduced pipeline was subsequently adapted for our own RMIT program information dataset and evaluation framework.

---

## Project Dataset

Our RAG system uses a custom evaluation dataset developed by Group 20 from RMIT program information.

The dataset consists of:

* **`passages_WIL20.csv`** — RMIT program information passages used as the knowledge base for retrieval
* **`topics_WIL20.csv`** — evaluation questions covering different programs, information needs and user personas
* **`qrels_WIL20.txt`** — relevance judgements mapping evaluation questions to relevant passages for retrieval evaluation

The question set includes multiple user personas, including prospective students, current students, and parents/guardians. It also includes intentionally unsupported questions to evaluate whether the chatbot appropriately abstains when the required information is not available in the knowledge base.

---

## RAG Evaluation Framework

The RAG system is evaluated at multiple stages to distinguish retrieval performance from response-generation performance.

### Retrieval Evaluation

BM25 retrieval is evaluated at **k = 3**, corresponding to the three passages supplied to the generator.

Metrics include:

* **Hit@3** — whether at least one relevant passage appears in the top three retrieved passages
* **Recall@3** — the proportion of relevant passages retrieved within the top three
* **NDCG@3** — evaluates the ranking of relevant passages while accounting for graded relevance
* **Persona-level analysis** — compares retrieval performance across prospective students, current students, and parents/guardians

### Generation Evaluation

Generated answers are evaluated using **DeepEval** with a locally hosted LLM judge.

Metrics include:

* **Answer Relevancy** — evaluates whether the generated response addresses the user's question
* **Faithfulness** — evaluates whether claims in the generated response are supported by the retrieved RMIT passages

Question-level results are retained to support qualitative error analysis and identify cases where retrieval succeeds but generation fails, such as incorrect interpretation of evidence or unnecessary abstention.

**Notebook:** `WIL20_Evaluation.ipynb` — contains retrieval evaluation, RAG generation, DeepEval evaluation, and question-level analysis.

---

## Evaluation Files

| File                        | Description                                                          |
| --------------------------- | -------------------------------------------------------------------- |
| `passages_WIL20.csv`        | RMIT information passages used as the RAG knowledge base             |
| `topics_WIL20.csv`          | Evaluation questions, personas and question metadata                 |
| `qrels_WIL20.txt`           | Relevance judgements used to evaluate BM25 retrieval                 |
| `WIL20_Evaluation.ipynb`    | Main notebook for retrieval and generation evaluation                |
| `Walert_Reproduction.ipynb` | Reproduction of the Walert RAG pipeline used as the project baseline |

