# scAtlasScout Skill

## Overview

**scAtlasScout** retrieves, ranks, and summarizes publicly available single-cell RNA-seq (scRNA-seq) and single-nucleus RNA-seq (snRNA-seq) datasets from GEO, SRA, and CellxGENE for any disease, tissue, or biological condition. It returns a ranked Markdown table with accession IDs, inferred sequencing platforms, and confidence scores. **scAtlasScout does NOT** perform downstream analysis, download raw data files, run alignment or quantification pipelines, or access any data behind authentication walls.

---

## Workflow at a Glance

1. **Query Expansion** — Parse the user's input, expand abbreviations (e.g., LUAD → lung adenocarcinoma), and generate a full set of synonyms to maximize recall across databases.
2. **Dataset Retrieval** — Search GEO, SRA, and CellxGENE using the expanded query terms, applying species and modality filters.
3. **Filtering** — Apply strict filters (species, modality, valid accession) and soft filters (platform preference, recency, cell count) as defined in `rules/filtering_rules.md`.
4. **Platform Inference & Ranking** — Infer the sequencing platform from available metadata using a confidence-weighted evidence hierarchy, then rank results by species, platform, cell count, and recency.
5. **Output Formatting** — Render a 9-column Markdown table with a query interpretation note, per-source counts, platform breakdown, and any warnings.

---

## Output Format

Results are returned as a Markdown table with the following 9 columns:

| Column | Description |
|---|---|
| `#` | Row number |
| `Accession` | Dataset accession ID (GSE, SRP, ERP, DRP, or CellxGENE UUID) |
| `Source` | Database of origin: GEO, SRA, or CellxGENE |
| `Title` | Dataset title as recorded in the source database |
| `Species` | Organism (Human / Mouse / Other) |
| `Platform` | Inferred sequencing platform (e.g., 10X Chromium, Smart-seq2) |
| `Confidence` | Platform inference confidence score (0.0 – 1.0) |
| `Cells/Samples` | Cell count (if available) or sample count with "(samples)" suffix |
| `Year` | Year of submission or publication |
| `DOI/Link` | Direct link to the dataset or associated publication |

A summary paragraph follows the table, reporting total counts by source, platform breakdown, and the top-ranked dataset.

---

## Supported Abbreviations

The skill recognizes 80+ disease and condition abbreviations. All are automatically expanded before search.

| Abbreviation | Full Name | Abbreviation | Full Name | Abbreviation | Full Name |
|---|---|---|---|---|---|
| ESCC | Esophageal squamous cell carcinoma | LUAD | Lung adenocarcinoma | LUSC | Lung squamous cell carcinoma |
| SCLC | Small cell lung cancer | NSCLC | Non-small cell lung cancer | GBM | Glioblastoma multiforme |
| HCC | Hepatocellular carcinoma | PDAC | Pancreatic ductal adenocarcinoma | TNBC | Triple-negative breast cancer |
| CRC | Colorectal cancer | AML | Acute myeloid leukemia | ALL | Acute lymphoblastic leukemia |
| CLL | Chronic lymphocytic leukemia | MM | Multiple myeloma | RCC | Renal cell carcinoma |
| HNSCC | Head and neck squamous cell carcinoma | OC | Ovarian cancer | EC | Endometrial cancer |
| BC | Breast cancer | PHEO | Pheochromocytoma | PANNET | Pancreatic neuroendocrine tumor |
| IBD | Inflammatory bowel disease | UC | Ulcerative colitis | CD | Crohn's disease |
| NASH | Non-alcoholic steatohepatitis | NAFLD | Non-alcoholic fatty liver disease | IPF | Idiopathic pulmonary fibrosis |
| AD | Alzheimer's disease | PD | Parkinson's disease | ALS | Amyotrophic lateral sclerosis |
| MS | Multiple sclerosis | SLE | Systemic lupus erythematosus | RA | Rheumatoid arthritis |
| T1D | Type 1 diabetes | T2D | Type 2 diabetes | CAD | Coronary artery disease |
| HF | Heart failure | CKD | Chronic kidney disease | FSGS | Focal segmental glomerulosclerosis |
| ATN | Acute tubular necrosis | DLBCL | Diffuse large B-cell lymphoma | FL | Follicular lymphoma |
| MCL | Mantle cell lymphoma | MDS | Myelodysplastic syndrome | MPN | Myeloproliferative neoplasm |
| CMML | Chronic myelomonocytic leukemia | GIST | Gastrointestinal stromal tumor | NPC | Nasopharyngeal carcinoma |
| HNC | Head and neck cancer | OSCC | Oral squamous cell carcinoma | CSCC | Cutaneous squamous cell carcinoma |
| BCC | Basal cell carcinoma | MEL | Melanoma | SKCM | Skin cutaneous melanoma |
| UVM | Uveal melanoma | RB | Retinoblastoma | MB | Medulloblastoma |
| ATRT | Atypical teratoid rhabdoid tumor | EPN | Ependymoma | PA | Pilocytic astrocytoma |
| LGG | Low-grade glioma | HGG | High-grade glioma | DIPG | Diffuse intrinsic pontine glioma |
| NB | Neuroblastoma | WT | Wilms tumor | RMS | Rhabdomyosarcoma |
| OS | Osteosarcoma | EWS | Ewing sarcoma | SS | Synovial sarcoma |
| LPS | Liposarcoma | MFH | Malignant fibrous histiocytoma | MPNST | Malignant peripheral nerve sheath tumor |
| ACC | Adrenocortical carcinoma | MTC | Medullary thyroid carcinoma | PTC | Papillary thyroid carcinoma |
| FTC | Follicular thyroid carcinoma | ATC | Anaplastic thyroid carcinoma | | |

---

## Scientific Caveats

- GEO and SRA metadata quality varies; platform inference from text is probabilistic
- CellxGENE only contains manually curated datasets; it is not exhaustive
- "Human" preference is a default; always note if mouse data is included
- Cell counts from GEO/SRA are estimated from sample counts; only CellxGENE provides exact cell counts
- Some datasets appear in both GEO and SRA; flag duplicates when title similarity > 0.9
- Datasets without a DOI may be preprints or unpublished; note this in the table

---

## Repository Structure

```
scAtlasScout-skill/
├── README.md                          # This file
├── skill.yaml                         # Skill definition and step declarations
├── prompts/
│   ├── dataset_search.md              # Reusable prompt template for dataset retrieval
│   ├── ontology_expansion.md          # Prompt for abbreviation and synonym expansion
│   └── harmonization.md              # Prompt for metadata normalization
├── rules/
│   ├── filtering_rules.md             # Strict and soft filtering criteria
│   └── platform_inference_rules.md   # Evidence hierarchy for platform detection
├── schemas/
│   └── output_schema.json            # JSON Schema (draft-07) for output records
├── examples/
│   ├── neuroendocrine_tumor.md        # Example: PANNET query
│   └── pancreas_snrnaseq.md          # Example: pancreatic cancer snRNA-seq query
└── README_assets/
    └── .gitkeep                       # Placeholder for future diagrams/images
```
