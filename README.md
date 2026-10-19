# CourceMate — COSC2669 WIL Project

**RMIT University | Group 20 | Case Studies in Data Science**

## 1. Project Overview

**CourceMate** is a proof-of-concept Retrieval-Augmented Generation (RAG) chatbot developed to help users access information about selected RMIT university programs.

The system retrieves relevant information from a curated knowledge base of official RMIT program content and uses a locally hosted large language model (LLM) to generate natural-language responses.

CourceMate was designed for three user personas:

- Prospective students
- Current students
- Parents and guardians

The project evaluates whether a RAG chatbot can retrieve relevant information, generate grounded answers, recognise unsupported questions, and communicate information clearly.

CourceMate is an academic prototype rather than an official RMIT advisory service. Its initial knowledge base focuses on selected programs, with potential for expansion following further evaluation and refinement.

## 2. Team Members

| Student ID | Name | Primary Technical Contributions |
|---|---|---|
| s4197741 | Ghislaine Olivar | RAG pipeline implementation, retrieval evaluation and refinement, DeepEval generation evaluation, Gradio interface |
| s4036275 | Sara Joshi | Robustness evaluation, out-of-KB classification and abstention analysis |
| s4204467 | Zois Stavrakas | Readability evaluation and response complexity analysis |
| s4191093 | Suzy Doan | Retrieval, generation and knowledge-base error analysis |

All members also contributed to project planning, documentation, presentations and reporting.

## 3. Project Scope

CourceMate's knowledge base covers selected RMIT programs from the School of Design and School of Economics, Finance and Marketing.

Programs represented include:

- Bachelor of Graphic Design
- Bachelor of Games
- Master of Animation, Games and Interactivity
- Bachelor of Business
- Bachelor of Commerce
- Bachelor of Laws/Bachelor of Commerce

The prototype focuses on common university information needs, including admission requirements, program structures, study options, prerequisites and related academic information.

The limited scope supports focused development and evaluation before potential expansion to additional RMIT programs.

## 4. Walert Reproduction

Before developing CourceMate, the team reproduced the RAG pipeline from [Walert](https://github.com/rmit-ir/walert), an RMIT university information chatbot.

The reproduction established a technical foundation for understanding retrieval, generation and evaluation.

**Initial implementation**

| Component | Approach |
|---|---|
| Retrieval | BM25 using `rank_bm25` |
| Generation | Ollama with `llama3.2:3b` |
| Prompting | Adapted from Walert's RAG approach |
| Dataset | 120 FAQ passages and 96 labelled test questions |

**Preliminary result:** The reproduced system achieved **67.71% top-3 retrieval accuracy (65/96 questions)** using Walert's relevance judgements.

The reproduction also revealed sensitivity to question wording and variation between generated responses.

**Notebook:** `Walert_Reproduction.ipynb`

The reproduced pipeline was subsequently adapted to the team's RMIT program dataset.

## 5. System Architecture

CourceMate follows a RAG workflow consisting of retrieval, generation and evaluation.

1. **Question input:** A user submits a natural-language question.
2. **Retrieval:** The system identifies the top three relevant knowledge-base passages.
3. **Generation:** A locally hosted LLM generates an answer using the retrieved context.
4. **Abstention:** The system is intended to decline questions when sufficient supporting information is unavailable.
5. **Evaluation:** Retrieval effectiveness, generation quality, robustness and readability are assessed.

### Technologies

| Component | Technology |
|---|---|
| Programming language | Python |
| Development environment | Jupyter Notebook |
| Data processing | pandas |
| Lexical retrieval | BM25 (`rank_bm25`) |
| Semantic retrieval | `nomic-embed-text` |
| Hybrid retrieval | Reciprocal Rank Fusion |
| Answer generation | Ollama (`llama3.2:3b`) |
| Generation evaluation | DeepEval |
| Local evaluation judge | `qwen3:1.7b` |
| User interface | Gradio |
| Version control | GitHub |
| Project management | Trello |

## 6. Project Dataset

The team developed a custom dataset using official RMIT program information.

### Dataset Files

| File | Description |
|---|---|
| `passages_WIL20.csv` | Curated RMIT program-information passages |
| `topics_WIL20.csv` | Evaluation questions, user personas and question metadata |
| `qrels_WIL20.txt` | Relevance judgements linking questions to supporting passages |

The knowledge base contains approximately 45 curated passages.

### Evaluation Questions

The corrected evaluation dataset contains **70 questions**.

| Persona | Answerable | Out-of-KB | Total |
|---|---:|---:|---:|
| Prospective student | 20 | 9 | 29 |
| Current student | 19 | 8 | 27 |
| Parent/guardian | 12 | 2 | 14 |
| **Total** | **51** | **19** | **70** |

Answerable questions assess whether the chatbot retrieves and communicates information supported by its knowledge base.

Out-of-knowledge-base (out-of-KB) questions assess whether the chatbot appropriately declines requests that cannot be answered from the available information.

## 7. Notebook Development History

The repository preserves successive Jupyter notebooks to document the incremental development of CourceMate.

| Stage | Notebook | Purpose |
|---|---|---|
| 1 | `Walert_Reproduction.ipynb` | Reproduces the Walert RAG pipeline |
| 2 | `WIL20_Evaluation.ipynb` | Implements the initial retrieval and generation evaluation |
| 3 | `WIL20_Evaluation_&_Readability.ipynb` | Integrates readability metrics |
| 4 | `WIL20_Evaluation_&_Readability_&OutofKB.ipynb` | Adds robustness and abstention evaluation |
| 5 | `WIL20_Evaluation_&_Readability_&OutofKB_&Semantic&Hybrid.ipynb` | Compares retrieval approaches and evaluates semantic retrieval |
| 6 | `WIL20_Evaluation_&_Readability_&OutofKB_&Semantic&Hybrid_Interface.ipynb` | Adds the Gradio demonstration interface |

### Recommended Notebook

For readers interested in the most complete implementation, begin with:

`WIL20_Evaluation_&_Readability_&OutofKB_&Semantic&Hybrid_Interface.ipynb`

Earlier notebooks are retained to demonstrate how the project evolved through evaluation and refinement.

### How to Navigate the Repository

| To examine... | Open... |
|---|---|
| Walert reproduction | `Walert_Reproduction.ipynb` |
| Initial retrieval and DeepEval | `WIL20_Evaluation.ipynb` |
| Readability evaluation | Readability notebook |
| Robustness and abstention testing | OutofKB notebook |
| BM25, semantic and hybrid comparison | Semantic&Hybrid notebook |
| Final integrated prototype and interface | Semantic&Hybrid_Interface notebook |
| Evaluation questions | `topics_WIL20.csv` |
| Knowledge-base content | `passages_WIL20.csv` |
| Relevance judgements | `qrels_WIL20.txt` |

The notebooks contain implementation code, evaluation procedures and saved outputs. Some stages require locally installed dependencies and Ollama models to rerun.

## 8. Evaluation Framework and Results

CourceMate was evaluated across four dimensions:

- **Retrieval:** Hit@3, Recall@3 and NDCG@3
- **Generation:** Answer Relevancy and Faithfulness
- **Robustness:** Overall accuracy, out-of-KB abstention and false abstention
- **Readability:** Flesch Reading Ease, Flesch-Kincaid Grade Level and other complexity measures

### 8.1 Retrieval Evaluation

Retrieval was evaluated on the 51 answerable questions.

| Method | Hit@3 | Recall@3 | NDCG@3 |
|---|---:|---:|---:|
| BM25 | 0.804 | 0.735 | 0.706 |
| Hybrid | 0.843 | 0.784 | 0.767 |
| **Semantic** | **0.882** | **0.843** | **0.781** |

Semantic retrieval achieved the strongest results across all three metrics.

Compared with BM25, semantic retrieval improved:

- Hit@3 by 7.8 percentage points
- Recall@3 by 10.8 percentage points
- NDCG@3 by 7.5 percentage points

The number of questions retrieving at least one relevant passage within the top three increased from **41/51 to 45/51**.

Semantic retrieval was therefore selected for the refined CourceMate pipeline.

### 8.2 Persona-Based Retrieval Evaluation

| Persona | Hit@3 | Recall@3 | NDCG@3 |
|---|---:|---:|---:|
| Current student | 0.947 | 0.895 | 0.831 |
| Prospective student | 0.850 | 0.825 | 0.750 |
| Parent/guardian | 0.833 | 0.792 | 0.754 |

Semantic retrieval improved performance particularly for prospective students and parents/guardians.

However, improvements were not uniform. Current-student Hit@3 remained unchanged compared with BM25, while NDCG@3 decreased slightly.

These findings demonstrate the importance of evaluating retrieval performance separately for different user groups.

### 8.3 Generation Evaluation

Generation quality was assessed using DeepEval on a stratified sample of **12 answerable questions**, comprising four questions from each persona.

| Metric | BM25 Baseline | Refined Semantic System |
|---|---:|---:|
| Answer Relevancy | 0.861 | 0.847 |
| Faithfulness | 0.972 | 1.000 |

Faithfulness improved in the refined evaluation, while Answer Relevancy decreased slightly.

The final Faithfulness score indicates that the evaluated responses were judged to be supported by their retrieved context.

However, these findings are preliminary because the sample was small and the metrics relied on an automated LLM judge. They do not guarantee factual accuracy across all possible questions.

### 8.4 Robustness Evaluation

Robustness was assessed using the complete 70-question evaluation set.

| Metric | Result |
|---|---:|
| Overall robustness accuracy | 88.6% |
| Out-of-KB abstention rate | 73.7% |
| False abstention rate | 5.9% |

The system appropriately declined 73.7% of unsupported questions.

However, **26.3% of unsupported questions received attempted answers**, demonstrating that abstention behaviour requires further improvement.

Out-of-KB abstention also varied across personas:

| Persona | Abstention Rate |
|---|---:|
| Current student | 75.0% |
| Prospective student | 66.7% |
| Parent/guardian | 100.0% |

The parent/guardian result was based on only two unsupported questions and should be interpreted cautiously.

### 8.5 Readability Evaluation

Readability analysis identified relatively complex generated responses.

| Metric | Result |
|---|---:|
| Flesch Reading Ease | 19.41 |
| Flesch-Kincaid Grade Level | 18.06 |
| Average words per sentence | 30.21 |
| Average syllables per word | 1.85 |
| Average complex words per answer | 7.25 |
| Gunning Fog Index | 21.05 |
| SMOG Index | 17.69 |

The low Flesch Reading Ease score suggests that responses may be difficult for users unfamiliar with university terminology.

Readability formulas provide approximate measures of text complexity rather than direct evidence of user comprehension.

## 9. Error Analysis and Iterative Refinement

Evaluation results informed changes to the initial implementation.

### Retrieval Failures

Preliminary BM25 error analysis identified cases where the system retrieved passages from incorrect programs, prioritised generic terminology or failed to identify the specific supporting evidence.

The refined semantic retriever improved overall retrieval performance, although six questions still failed to retrieve a relevant passage within the top three results. Five of these involved Bachelor of Games questions.

### Generation Failures

Manual inspection demonstrated that successful retrieval does not always guarantee a correct generated response.

For example, question C42Q01 involved study-duration information that was incorrectly interpreted by the generator, illustrating the importance of evaluating generation separately from retrieval.

### Knowledge-Base Gaps

Question C25Q01 was reclassified as out-of-KB after manual inspection determined that the available passages did not contain sufficient information to answer it.

### Evaluation Reproducibility

Repeated generation produced variations in evaluation scores. To improve reproducibility, generated answers were saved before subsequent scoring.

### Computational Constraints

The team initially planned to conduct DeepEval evaluation on all 51 answerable questions.

However, local computational constraints made this impractical. A stratified sample of 12 questions was used instead.

This decision balanced evaluation coverage with available resources while introducing a limitation to the generalisability of the generation results.

## 10. Gradio Demonstration Interface

A Gradio interface was developed to provide a simple user-facing demonstration of CourceMate.

The interface allows users to submit natural-language questions and view generated answers without interacting directly with the notebook code.

The interface was incorporated into the final integrated notebook and supported the project's oral presentation.

## 11. Project Management

The team used [Trello](https://trello.com/b/iF82apWt/cosc2669-wil-project) to coordinate weekly tasks and monitor development progress.

Weekly mentoring meetings were scheduled for **Tuesdays at 1:00 pm**.

The project progressed through:

1. Requirements gathering and Walert reproduction
2. Knowledge-base construction and question development
3. Initial RAG implementation and evaluation
4. Error analysis and retrieval refinement
5. Robustness and readability integration
6. Gradio interface development and demonstration
7. Final reporting and reflection

The repository preserves successive notebooks and development artifacts as evidence of incremental progress.

## 12. Limitations and Future Work

CourceMate is a proof-of-concept prototype and has not been validated for university-wide deployment.

Potential future improvements include:

- **Expanding knowledge-base coverage** to additional RMIT programs after further validation.
- **Addressing program-specific retrieval failures**, particularly those involving Bachelor of Games.
- **Improving abstention behaviour** to reduce unsupported answers.
- **Simplifying generated responses** through shorter sentences and more accessible language.
- **Conducting broader generation evaluations** with a larger question sample.
- **Introducing user testing** to assess practical usefulness, comprehension and user experience.

## 13. References and Acknowledgements

This project builds on the team's reproduction of [Walert](https://github.com/rmit-ir/walert), developed by researchers at RMIT University.

The associated research paper is referenced in the WIL written report.

The team acknowledges the guidance provided through the COSC2669 WIL mentoring sessions.

## Disclaimer

**CourceMate is an academic prototype developed for COSC2669 at RMIT University.**

It is not an official RMIT advisory service. Program information and generated responses should be verified against official RMIT sources before being used for enrolment or academic decisions.
