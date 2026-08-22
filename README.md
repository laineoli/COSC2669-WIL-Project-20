# COSC2669-WIL-Project-20
# Group ID: 20

## Team Members

| Student ID | Full Name |
|---|---|
|s4197741 | Ghislaine Olivar |
|s4036275 | Sara Joshi |
|s4204467 | Zois Stavrakas |
|s4191093 | Suzy Doan |

## Walert Reproduction

We reproduced the RAG pipeline from [Walert](https://github.com/rmit-ir/walert) (RMIT's FAQ chatbot for the School of Technologies) as a starting point for our own project.

**Approach:**
- **Retrieval:** BM25 (via `rank_bm25`), following Walert's lexical retrieval baseine
- **Generation:** Ollama (llama3.2:3b), run locally at no cost, using a prompt structure adapted from Walert's `RAG_SYSTEM.py`
- **Validation:** Pipeline tested end-to-end against Walert's own dataset (120 FAG passages, 96 labelled test questions)

**Preliminary results:**
- 67.71% top-3 retrieval accuracy (65/96 questions), evaluated against Walert's provided relevance judgements (`qrels.txt`)
- Generation produces grounded answers when retrieval succeeds, but is sensitive to question phrasing where different paraphrasings of the same underlying question sometimes retrieved different passages
- Observed run-to-run variability from local LLm sampling, including one case where the system correctly returned "NA" rather than hallucinating an answer

**Notebook:** [`Walert_Reproduction.ipynb`](./Walert_Reproduction.ipynb) - contains full pipeline: data loading, BM25 indexing, retrieval evaluation and generation testing

**Next steps:** Adopting this pipeline into our own RMIT knowledge base of course handbooks and admissions materials, with an evaluation framework of our own chosen evaluation metrics.
