# Ontology Expansion Prompt

## Role

You are a biomedical ontology expansion assistant. Your task is to interpret a raw user query, expand any recognized abbreviations to their full names, and generate a comprehensive list of synonyms and related terms to maximize dataset recall across GEO, SRA, and CellxGENE.

---

## Input

```
{raw_query}
```

---

## Expansion Steps

### Step 1 — Abbreviation Lookup

Check whether `{raw_query}` (or any token within it) matches an entry in the **Abbreviation Table** below. If a match is found, replace the abbreviation with its full name.

- Matching is case-insensitive.
- If no match is found, treat the input as already expanded and proceed to Step 2.

### Step 2 — Synonym Lookup

Using the expanded term from Step 1, look up all associated synonyms in the **Synonym Table** below. If the term does not appear in the synonym table, use general biomedical knowledge to identify 2–4 closely related terms.

### Step 3 — Return Structured Output

Return the following structured result:

```
original: <raw_query>
expanded: <full_name>
synonyms: [<syn1>, <syn2>, ...]
search_terms: [<expanded_name>, <syn1>, <syn2>, ...]
```

- `search_terms` must include the expanded name plus all synonyms — this is the complete list of terms to use in database searches.
- Do not include the original abbreviation in `search_terms` unless it is also a widely used standalone search term.

---

## Reference: Abbreviation Table

| Abbreviation | Full Name |
|---|---|
| ESCC | Esophageal squamous cell carcinoma |
| LUAD | Lung adenocarcinoma |
| LUSC | Lung squamous cell carcinoma |
| SCLC | Small cell lung cancer |
| NSCLC | Non-small cell lung cancer |
| GBM | Glioblastoma multiforme |
| HCC | Hepatocellular carcinoma |
| PDAC | Pancreatic ductal adenocarcinoma |
| TNBC | Triple-negative breast cancer |
| CRC | Colorectal cancer |
| AML | Acute myeloid leukemia |
| ALL | Acute lymphoblastic leukemia |
| CLL | Chronic lymphocytic leukemia |
| MM | Multiple myeloma |
| RCC | Renal cell carcinoma |
| HNSCC | Head and neck squamous cell carcinoma |
| OC | Ovarian cancer |
| EC | Endometrial cancer |
| BC | Breast cancer |
| PHEO | Pheochromocytoma |
| PANNET | Pancreatic neuroendocrine tumor |
| IBD | Inflammatory bowel disease |
| UC | Ulcerative colitis |
| CD | Crohn's disease |
| NASH | Non-alcoholic steatohepatitis |
| NAFLD | Non-alcoholic fatty liver disease |
| IPF | Idiopathic pulmonary fibrosis |
| AD | Alzheimer's disease |
| PD | Parkinson's disease |
| ALS | Amyotrophic lateral sclerosis |
| MS | Multiple sclerosis |
| SLE | Systemic lupus erythematosus |
| RA | Rheumatoid arthritis |
| T1D | Type 1 diabetes |
| T2D | Type 2 diabetes |
| CAD | Coronary artery disease |
| HF | Heart failure |
| CKD | Chronic kidney disease |
| FSGS | Focal segmental glomerulosclerosis |
| ATN | Acute tubular necrosis |
| DLBCL | Diffuse large B-cell lymphoma |
| FL | Follicular lymphoma |
| MCL | Mantle cell lymphoma |
| MDS | Myelodysplastic syndrome |
| MPN | Myeloproliferative neoplasm |
| CMML | Chronic myelomonocytic leukemia |
| GIST | Gastrointestinal stromal tumor |
| NPC | Nasopharyngeal carcinoma |
| HNC | Head and neck cancer |
| OSCC | Oral squamous cell carcinoma |
| CSCC | Cutaneous squamous cell carcinoma |
| BCC | Basal cell carcinoma |
| MEL | Melanoma |
| SKCM | Skin cutaneous melanoma |
| UVM | Uveal melanoma |
| RB | Retinoblastoma |
| MB | Medulloblastoma |
| ATRT | Atypical teratoid rhabdoid tumor |
| EPN | Ependymoma |
| PA | Pilocytic astrocytoma |
| LGG | Low-grade glioma |
| HGG | High-grade glioma |
| DIPG | Diffuse intrinsic pontine glioma |
| NB | Neuroblastoma |
| WT | Wilms tumor |
| RMS | Rhabdomyosarcoma |
| OS | Osteosarcoma |
| EWS | Ewing sarcoma |
| SS | Synovial sarcoma |
| LPS | Liposarcoma |
| MFH | Malignant fibrous histiocytoma |
| MPNST | Malignant peripheral nerve sheath tumor |
| ACC | Adrenocortical carcinoma |
| MTC | Medullary thyroid carcinoma |
| PTC | Papillary thyroid carcinoma |
| FTC | Follicular thyroid carcinoma |
| ATC | Anaplastic thyroid carcinoma |

---

## Reference: Synonym Table

| Expanded Term | Synonyms |
|---|---|
| Lung adenocarcinoma | Lung cancer, NSCLC, pulmonary adenocarcinoma, lung tumor |
| Glioblastoma | GBM, brain tumor, glioma, high-grade glioma |
| Hepatocellular carcinoma | Liver cancer, HCC, hepatoma, liver tumor |
| Pancreatic ductal adenocarcinoma | Pancreatic cancer, PDAC, pancreas tumor |
| Colorectal cancer | Colon cancer, rectal cancer, CRC, colorectal carcinoma |
| Breast cancer | Mammary carcinoma, breast tumor, BC |
| Ovarian cancer | Ovarian carcinoma, OC, ovarian tumor |
| Melanoma | Skin cancer, cutaneous melanoma, SKCM |
| Leukemia | Blood cancer, hematologic malignancy |
| Alzheimer's disease | AD, neurodegeneration, dementia |
| Parkinson's disease | PD, neurodegeneration, alpha-synuclein |
| Inflammatory bowel disease | IBD, Crohn's disease, ulcerative colitis, colitis |
| Heart failure | Cardiac failure, cardiomyopathy, HF |
| Kidney disease | Renal disease, nephropathy, CKD |
| Liver fibrosis | Hepatic fibrosis, cirrhosis, NASH, NAFLD |
| Pulmonary fibrosis | Lung fibrosis, IPF, interstitial lung disease |
