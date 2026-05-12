# Example: Pancreatic Cancer snRNA-seq Query

## Input Query

```
Find snRNA-seq datasets for pancreatic cancer, human only
```

---

## Step 1 — Query Expansion

**Abbreviation lookup**: "pancreatic cancer" is a full-form term; no abbreviation expansion needed.

**Synonym expansion**:

```
original:      pancreatic cancer
expanded:      pancreatic ductal adenocarcinoma
synonyms:      [PDAC, pancreatic cancer, pancreas tumor, pancreatic carcinoma, exocrine pancreatic cancer]
search_terms:  [pancreatic ductal adenocarcinoma, PDAC, pancreatic cancer, pancreas tumor,
                pancreatic carcinoma, exocrine pancreatic cancer]
```

**Query interpretation note** (prepended to output):
> **Query interpretation**: "pancreatic cancer" → pancreatic ductal adenocarcinoma (+ synonyms: PDAC, pancreas tumor, pancreatic carcinoma)

---

## Step 2 — Search Parameters Applied

| Parameter | Value |
|---|---|
| Disease terms | pancreatic ductal adenocarcinoma, PDAC, pancreatic cancer, pancreas tumor, pancreatic carcinoma |
| Species | Human (explicitly requested) |
| Modality | **snRNA-seq only** (explicitly requested) |
| Platform preference | 10X Chromium (default) |
| Result count | 10 per source (default) |

**Modality filter applied**: Only records with inferred or stated modality `snRNA-seq` are included. Records identified as scRNA-seq are excluded at the filtering step.

---

## Expected Output Table

> **Query interpretation**: "pancreatic cancer" → pancreatic ductal adenocarcinoma (+ synonyms: PDAC, pancreas tumor, pancreatic carcinoma)
> **Modality filter**: snRNA-seq only

| # | Accession | Source | Title | Species | Platform | Confidence | Cells/Samples | Year | DOI/Link |
|---|---|---|---|---|---|---|---|---|---|
| 1 | GSE197177 | GEO | Single-nucleus RNA-seq of human pancreatic ductal adenocarcinoma and adjacent normal pancreas | Human | 10X Chromium | 0.8 | 28 samples | 2022 | https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE197177 |
| 2 | a4b7c2d1-3e5f-4a9b-8c0d-2e3f4a5b6c7d | CellxGENE | snRNA-seq atlas of human PDAC tumor microenvironment | Human | 10X Chromium | 0.9 | 112,540 cells | 2023 | https://cellxgene.cziscience.com/datasets/a4b7c2d1-3e5f-4a9b-8c0d-2e3f4a5b6c7d |
| 3 | SRP334821 | SRA | Single-nucleus transcriptomics of pancreatic cancer stroma and epithelium | Human | 10X Chromium | 0.6 | 16 samples | 2022 | https://www.ncbi.nlm.nih.gov/sra/?term=SRP334821 |
| 4 | GSE178341 | GEO | snRNA-seq characterization of PDAC subtypes in treatment-naive patients | Human | 10X Chromium | 0.8 | 22 samples | 2021 | https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE178341 |
| 5 | GSE162708 | GEO | Nucleus-level transcriptomics of human pancreatic tumors and matched normal tissue | Human | Unknown | 0.0 | 8 samples | 2021 | https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE162708 |

---

## Summary

Found **5 datasets** (3 from GEO, 1 from SRA, 1 from CellxGENE) matching the snRNA-seq modality filter for pancreatic cancer in human samples.
Platform breakdown: 10X Chromium (4), Unknown (1).
Top dataset: `a4b7c2d1-...` — snRNA-seq atlas of human PDAC tumor microenvironment (112,540 cells, 2023).

> **Note**: An additional 8 scRNA-seq datasets were found but excluded due to the snRNA-seq-only modality filter. To include those results, rerun the query with `modality = "both"`. GSE197177 has a linked SRA study accession — potential cross-source duplicate flagged. GSE162708 has no associated DOI; it may be an unpublished or preprint-linked submission.
