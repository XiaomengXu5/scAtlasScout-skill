# Filtering Rules

These rules govern which dataset records are included in, penalized within, or excluded from the final ranked output. Rules are divided into **Strict Filters** (mandatory exclusion criteria) and **Soft Filters** (ranking penalties). Duplicate detection and failure handling are also defined here.

---

## Strict Filters

Records that violate any strict filter are **excluded entirely** from the output.

### SF-1: Species

- **Rule**: The dataset organism must be *Homo sapiens* (Human).
- **Exception**: If the user explicitly requests mouse data (e.g., "include mouse datasets" or "mouse only"), *Mus musculus* records are also permitted.
- **Action on violation**: Exclude the record. Do not include it in the results table.

### SF-2: Modality

- **Rule**: The dataset modality must be scRNA-seq or snRNA-seq (or both, if `modality = "both"` was requested).
- **Excluded modalities**: bulk RNA-seq, ATAC-seq, ChIP-seq, proteomics, spatial transcriptomics (unless explicitly requested), methylation arrays, and any other non-single-cell modality.
- **Action on violation**: Exclude the record.

### SF-3: Valid Accession ID

- **Rule**: The dataset must have a non-empty accession ID matching one of the following formats:
  - GEO: `GSE` followed by digits (e.g., `GSE123456`)
  - SRA study: `SRP`, `ERP`, or `DRP` followed by digits (e.g., `SRP098765`)
  - CellxGENE: a valid UUID (e.g., `3a928d6b-...`)
- **Action on violation**: Exclude the record. A missing or malformed accession cannot be reliably cited or retrieved.

---

## Soft Filters

Records that fail a soft filter are **retained but ranked lower**. Apply a ranking penalty for each soft filter violated.

### SoF-1: Platform Preference

- **Rule**: Datasets using the user's preferred platform (default: 10X Chromium) are ranked higher.
- **Penalty**: Datasets using other known platforms are ranked below 10X Chromium datasets. Datasets with `platform = "Unknown"` are ranked lowest.

### SoF-2: Minimum Cell / Sample Count

- **Rule**: Prefer datasets with ≥ 1,000 cells or ≥ 5 samples.
- **Penalty**: Datasets below this threshold are ranked lower. They are not excluded, as small pilot datasets may still be scientifically relevant.

### SoF-3: Recency

- **Rule**: Prefer datasets submitted or published in 2019 or later.
- **Penalty**: Datasets from before 2019 are ranked lower. Very old datasets (pre-2016) receive a stronger penalty.

### SoF-4: Data Availability

- **Rule**: Prefer datasets that have raw count matrices available (e.g., barcodes.tsv, matrix.mtx, features.tsv for 10X; count matrix files for other platforms).
- **Penalty**: Datasets with no downloadable count data are ranked lower.

---

## Duplicate Detection

Duplicates are **flagged**, not removed. The `is_duplicate_flag` field in the output schema is set to `true` for flagged records, and a note is added explaining the suspected duplicate.

### Duplicate Condition 1: Cross-Source Title Match

- Flag a record as a potential duplicate if its title has > 90% string similarity to another record from a different source (e.g., the same study appears in both GEO and SRA).
- Add a note: `"Potential duplicate of <accession> (<source>)"`.

### Duplicate Condition 2: Shared GEO Accession

- Flag a record as a potential duplicate if the same GSE accession number appears in both GEO and SRA results (SRA studies are often linked to GEO submissions).
- Add a note: `"GSE accession also present in SRA results"`.

### Handling Flagged Duplicates

- Include both records in the output table.
- Display the duplicate flag visually (e.g., a note in the `Notes` column).
- Do not automatically remove either record — the user decides which to use.

---

## Failure Handling

These rules apply when a database source is unavailable or returns no results.

### FH-1: Source Returns Zero Results

- **Action**: Include a note in the output summary: `"No results from <source> for this query."`
- Continue processing results from other sources normally.

### FH-2: Source Returns an Error

- **Action**: Include a warning at the top of the output: `"⚠️ <source> unavailable — results may be incomplete."`
- Skip that source and continue with the remaining sources.
- Do not halt execution or return an empty response.

### FH-3: All Sources Fail

- **Action**: Return a response explaining that all sources were unavailable, with the warning messages for each. Do not return an empty response.

### FH-4: Partial Results

- **Rule**: Never return an empty results table if any records passed the strict filters.
- Always return whatever valid results are available, even if fewer than `result_count`.
