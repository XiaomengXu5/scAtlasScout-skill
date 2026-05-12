# Example: Neuroendocrine Tumor Query

## Input Query

```
Find single-cell datasets for PANNET
```

---

## Step 1 — Query Expansion

**Abbreviation lookup**: `PANNET` → **pancreatic neuroendocrine tumor**

**Synonym expansion**:

```
original:      PANNET
expanded:      pancreatic neuroendocrine tumor
synonyms:      [neuroendocrine tumor, NET, islet cell tumor, pancreatic NET, pNET, PanNET]
search_terms:  [pancreatic neuroendocrine tumor, neuroendocrine tumor, NET, islet cell tumor,
                pancreatic NET, pNET, PanNET]
```

**Query interpretation note** (prepended to output):
> **Query interpretation**: "PANNET" → pancreatic neuroendocrine tumor (+ synonyms: neuroendocrine tumor, NET, islet cell tumor, pancreatic NET)

---

## Step 2 — Search Parameters Applied

| Parameter | Value |
|---|---|
| Disease terms | pancreatic neuroendocrine tumor, neuroendocrine tumor, NET, islet cell tumor, pancreatic NET |
| Species | Human (default) |
| Modality | scRNA-seq + snRNA-seq (default) |
| Platform preference | 10X Chromium (default) |
| Result count | 10 per source (default) |

---

## Expected Output Table

> **Query interpretation**: "PANNET" → pancreatic neuroendocrine tumor (+ synonyms: neuroendocrine tumor, NET, islet cell tumor, pancreatic NET)

| # | Accession | Source | Title | Species | Platform | Confidence | Cells/Samples | Year | DOI/Link |
|---|---|---|---|---|---|---|---|---|---|
| 1 | GSE183795 | GEO | Single-cell transcriptomic landscape of pancreatic neuroendocrine tumors | Human | 10X Chromium | 0.8 | 32 samples | 2022 | https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE183795 |
| 2 | GSE155698 | GEO | scRNA-seq of human pancreatic islet tumors and adjacent normal tissue | Human | 10X Chromium | 0.6 | 18 samples | 2021 | https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE155698 |
| 3 | SRP312047 | SRA | Transcriptomic profiling of neuroendocrine tumors at single-cell resolution | Human | 10X Chromium | 0.6 | 24 samples | 2022 | https://www.ncbi.nlm.nih.gov/sra/?term=SRP312047 |
| 4 | 7f3c9a1b-2e4d-4f8a-b6c0-1d2e3f4a5b6c | CellxGENE | Human pancreatic neuroendocrine tumor single-cell atlas | Human | 10X Chromium | 0.9 | 87,412 cells | 2023 | https://cellxgene.cziscience.com/datasets/7f3c9a1b-2e4d-4f8a-b6c0-1d2e3f4a5b6c |
| 5 | GSE141017 | GEO | Single-nucleus RNA sequencing of pancreatic NET and normal islets | Human | Unknown | 0.0 | 12 samples | 2020 | https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE141017 |

---

## Summary

Found **5 datasets** (2 from GEO, 1 from SRA, 1 from CellxGENE; 1 additional GEO record with snRNA-seq modality).
Platform breakdown: 10X Chromium (4), Unknown (1).
Top dataset: `7f3c9a1b-...` — Human pancreatic neuroendocrine tumor single-cell atlas (87,412 cells, 2023).

> **Note**: GSE183795 also appears in SRA results under a linked study accession — potential duplicate flagged. No datasets without a DOI were returned; all records link to publicly accessible database pages.
