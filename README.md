# ✂Paragraph Similarity with Shingling, MinHash & LSH

This project finds the **most similar paragraph pairs** in a text file using a classic **locality-sensitive hashing (LSH)** pipeline:

1) **Shingling** – tokenize paragraphs and create k-word shingles  
2) **MinHash** – compress each paragraph’s shingle set into a compact signature using random permutations  
3) **LSH (bands/rows)** – hash signature bands into buckets to quickly surface likely-similar pairs  
4) **Ranking** – compute a similarity score and return the **Top-K** most similar paragraph pairs

> Input is a single file (`similarity.txt`) where **paragraphs are separated by a blank line**.

---

## What this repo does

- Reads `similarity.txt` and splits into paragraphs  
- Builds **1-gram shingles** (configurable if you extend `shingle(..., k)`)  
- Assigns integer IDs to shingles and builds **bit vectors** per paragraph  
- Generates **100 MinHash permutations**  
- Uses **20 bands** to perform **LSH candidate generation**  
- Scores candidate pairs and outputs the **Top 5 most similar paragraph pairs**

---

## Methods in this implementation

- **Shingling**  
  - Lowercases and strips punctuation via regex  
  - Default `k = 1` (unigrams). Extendable to bigrams/trigrams.

- **MinHash**  
  - Creates 100 random permutations of shingle IDs  
  - Signature entry = first permuted index where the bit vector has a 1

- **LSH (Bands/Rows)**  
  - Splits each signature into **20 bands** (equal rows per band)  
  - Hashes each band (via MD5 of the list) to a bucket  
  - Any bucket with ≥2 items yields **candidate pairs**

- **Scoring**  
  - Current code ranks pairs by **overlap of MinHash signature values** (set Jaccard on signatures)  
  - (Optional improvement below: re-score using **exact Jaccard** on original shingle sets for more fidelity)




