
# CS 5588 Data Science Capstone: RAG Hands-On Write-up

## Problem Motivation
Retrieval-Augmented Generation (RAG) methods aim to improve large language model (LLM) output accuracy and relevance by grounding answers in external knowledge sources. This avoids hallucination and enables explainability, which is critical in domains like finance and research where factual correctness is paramount.

***

## Method Summary
This project utilized LangChain, Chroma, and Gemini APIs to build end-to-end RAG pipelines. The methodology included:

- **Tools:** LangChain framework for chaining, Chroma for vector storage and retrieval, Gemini and HuggingFace LLMs for generation.
- **Embeddings:** SentenceTransformers MiniLM (`all-MiniLM-L6-v2`) and E5-small (`intfloat/e5-small-v2`) embedding models were tested.
- **Chunking:** Document texts were split using RecursiveCharacterTextSplitter with chunk sizes primarily 500 characters with 100 overlap, and smaller chunk experiments at 300 characters with 50 overlap.
- **Fine-Tuning:** Track C extended this by exploring Gemini LLM fine-tuning on the financial dataset for enhanced task-specific performance.
- **Reproducibility:** Environment and pipeline configuration logging via `env_rag.json` and `rag_run_config.json`.

***

## Results and Mini-Experiment Insights

### Track A (LangChain with Gemini)
- **Embedding Swap** showed MiniLM offered concise, high-level summaries while E5-small provided more granular, technical answers.
- **Chunk Sensitivity** revealed larger chunks delivered broader context and synthesis, whereas smaller chunks yielded focused, precise answers.
- These insights guided vector store and retrieval design for accuracy and recall balance.

### Track B (Gemini embeddings vs MiniLM embeddings)
- Retrieval of paper1 contributions failed across embeddings due to incomplete context indexing but models correctly declined to hallucinate answers.
- Chunk granularity markedly impacted retrieval:
  - Default 500-char chunks missed some specific papers.
  - Smaller 300-char chunks enabled precise retrieval like summarizing a paper analyzing CBDC narratives with LLMs.
- This reinforced the need for careful preprocessing and chunk size tuning in RAG pipelines.

### Track C (Gemini Fine-Tuning)
- Fine-tuning Gemini on domain-specific finance data improved relevance and factuality of generated responses.
- Enhanced performance was demonstrated in qualitative tests against baseline Gemini and HuggingFace models.
- Integration with LangChain pipelines provided seamless infrastructure for fine-tuning, embedding swap, and chunk sensitivity experiments.

***

## Challenges and Mitigation
- **Environment/Dependency Issues:** Encountered during initial setup; resolved by careful version pinning and environment logging.
- **Data Coverage:** Missing or malformed chunks degraded retrieval accuracy; mitigated by iterative chunk size tuning and document validation.
- **API & Rate Limits:** Gemini API constraints required batch testing and async handling to avoid throttling.
- **Reproducibility:** Automated environment and run-configuration logging ensured full experiment repeatability.

***

## Reproducibility Checklist

1. Clone the GitHub repo containing notebooks and artifacts.
2. Install dependencies 
3. Upload your documents to `data/` folder or use provided sample data.
4. Export your Gemini API key as environment variable:
   ```
   export GEMINI_API_KEY="your_api_key_here"
   ```
5. Run notebooks end-to-end, starting with environment logging.
6. Check that `env_rag.json` and `rag_run_config.json` are generated.
7. Validate mini-experiment cells for embedding swaps and chunk sensitivity.
8. Review outputs, logs, and source mappings for transparency.

