# Hybride-Product_Search-Engine

Keyword search breaks the moment a user types something slightly off — a typo, an abbreviation, or just a different word for the same thing. This project is an attempt to fix that for financial product catalogues. You type "scf" or "working capital loan" or even "ladies business account", and it finds the right product. Not because it's doing a simple string match, but because it understands what you probably meant.

---

**What it actually does**

The search runs every query through a pipeline — abbreviation expansion, synonym resolution, POS-aware lemmatisation, fuzzy matching, phrase matching, and finally, semantic similarity via a sentence transformer model. Each product gets scored across nine signals and ranked by a weighted combined score.

The reason it's built this way is that any single method fails on its own. Pure fuzzy matching returns garbage for short queries. Pure semantic search is too slow and returns vague results for domain-specific terms. The hybrid approach handles the edge cases that matter in practice.

---

**Tech used**

- `sentence-transformers` with `BAAI/bge-base-en-v1.5` for semantic similarity
- `thefuzz` for fuzzy token matching
- `nltk` for POS tagging and lemmatization
- `sqlite3` / `oracledb` for data layer
- `ipywidgets` for the interactive search UI in the notebook

---

**Before you run this**

The notebook has no product data in it — you'll need to bring your own. Section 3 has two loading options: a CSV path you can point to your own file, and a commented-out database block for Oracle connections using environment variables.

Your data needs, at a minimum, a product name column and a description column. The richer the descriptions, the better the semantic results.

You'll also want to update the abbreviation and synonym maps in Section 2 to match your own domain's terminology. Those mappings are what search feels "smart" for a specific catalogue — without them, it's just a generic fuzzy matcher.

---

**Model setup**

Point `model_path` in Section 5 to a local download of `BAAI/bge-base-en-v1.5`, or swap it for the one-line auto-download:

```python
model = SentenceTransformer("BAAI/bge-base-en-v1.5")
```

---
