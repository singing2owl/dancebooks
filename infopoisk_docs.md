# Dance Library Search Tool Documentation

This tool performs interactive search queries on a corpus of dance-related bibliographic data. It supports three search algorithms: BM25 (keyword-based) and two semantic search methods (Word2Vec and SBERT).

## Prerequisites

- Python 3.x installed
- Virtual environment set up in `.venv` folder
- Required dependencies installed (see `requirements.txt`)

## Activation

Activate the virtual environment before running the tool:

- **Command Prompt (cmd)**: `.venv\Scripts\activate.bat`
- **PowerShell**: `.venv\Scripts\Activate.ps1`

## Usage

Run the interactive CLI:

```
python infopoisk_cli.py
```

The tool runs as an interactive session with the following steps:

**Step 1 — Corpus loading**
The tool automatically parses all `.bib` files and builds the document corpus. No input is required.

**Step 2 — Search type selection**
Choose one of three search algorithms:

| Option | Algorithm  | Description |
|--------|------------|-------------|
| `1`    | BM25       | Classical keyword search — fast and precise |
| `2`    | Word2Vec   | Semantic search using GloVe vectors — English only |
| `3`    | SBERT      | Semantic search using Sentence-Transformers — multilingual (50+ languages) |

**Step 3 — Enter a query**
Type a search query in any language supported by the chosen algorithm. Example: `dance history`, `ballet`, `folk dance`.

**Step 4 — Choose number of results**
Enter how many top results to return (default: `5`).

After results are displayed, the tool asks whether to run another search. Enter `y` to continue or `n` to exit.

## Output

Results are printed to the console in the format:

```
doc_id: score
```

Higher scores indicate better matches. Search time is also reported after each query.

## Search Algorithm Details

**BM25** — Lemmatizes both the corpus and query, then scores documents using the BM25 ranking function. Best for exact or near-exact keyword matches. Fast and requires no external model downloads.

**Word2Vec (GloVe)** — Represents documents and queries as mean word vectors using the `glove-wiki-gigaword-100` model. Enables semantic matching (e.g. "waltz" matching "ballroom"). English-only due to the underlying model.

**SBERT** — Encodes full document strings and the query using the `paraphrase-multilingual-MiniLM-L12-v2` Sentence-Transformers model (~120 MB, downloaded on first use). Produces the best semantic results and supports all corpus languages including Russian, French, German, Italian, Spanish, and more.

## Notes

- All `.bib` files in the configured `bibdata_dir` are processed automatically.
- BM25 and Word2Vec use lemmatized tokens; SBERT uses the original raw text.
- Each document is lemmatized using the languages appropriate to its source file (as defined in the language mapping in `infopoisk_data_prep.py`).
- Queries for BM25 and Word2Vec are lemmatized using Russian and English by default.
- No results are shown if the query matches no documents.
- SBERT and Word2Vec models are downloaded automatically on first use and cached locally.