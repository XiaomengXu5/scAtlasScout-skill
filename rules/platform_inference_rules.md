# Platform Inference Rules

These rules define how to infer the sequencing platform for a dataset when it is not explicitly stated, and how to resolve conflicts when multiple evidence sources disagree.

---

## Evidence Hierarchy

Apply evidence sources in priority order. Use the highest-priority evidence available. Assign the corresponding confidence score and platform label.

| Priority | Evidence | Confidence | Platform Label |
|---|---|---|---|
| 1 | Explicit platform field (CellxGENE `assay` field, SRA `PLATFORM` tag) | 0.9 | As stated in the field |
| 2 | Title contains "10x", "10X Genomics", or "Chromium" | 0.8 | 10X Chromium |
| 3 | Title contains "Smart-seq2" or "Smart-seq" | 0.8 | Smart-seq2 |
| 4 | Title contains "Drop-seq" | 0.8 | Drop-seq |
| 5 | Title contains "inDrops" | 0.8 | inDrops |
| 6 | Title contains "MARS-seq" | 0.8 | MARS-seq |
| 7 | Title contains "sci-RNA-seq" | 0.8 | sci-RNA-seq |
| 8 | Title contains "Microwell-seq" | 0.8 | Microwell-seq |
| 9 | Supplementary files include `barcodes.tsv`, `matrix.mtx`, and `features.tsv` | 0.7 | 10X Chromium |
| 10 | Summary or abstract contains "10x", "Chromium", or "Cell Ranger" | 0.6 | 10X Chromium |
| 11 | No platform evidence found in any field | 0.0 | Unknown |

### Notes on Priority 1

When an explicit platform field is present, use the value as stated. Normalize common variants:
- "10x Chromium", "10X", "Chromium v2", "Chromium v3", "10x Genomics Chromium" → `10X Chromium`
- "Smart-seq 2", "Smartseq2", "SMART-Seq v4" → `Smart-seq2`
- "Dropseq", "Drop-Seq" → `Drop-seq`

---

## Conflict Resolution

### Rule CR-1: Highest Confidence Wins

If evidence from multiple sources points to different platforms, use the evidence with the highest confidence score. Discard lower-confidence conflicting evidence.

**Example**: An explicit CellxGENE assay field (confidence 0.9) says "Smart-seq2", but the title contains "10x" (confidence 0.8). Use Smart-seq2 with confidence 0.9.

### Rule CR-2: Equal Confidence — Prefer Specificity

If two evidence sources have equal confidence and agree on the platform family but differ in specificity, prefer the more specific label.

**Example**: One source indicates "10X Chromium" (confidence 0.8) and another indicates "10X Chromium v3" (confidence 0.8). Use `10X Chromium v3`.

### Rule CR-3: Equal Confidence — Irreconcilable Conflict

If two evidence sources have equal confidence and point to genuinely different platforms (e.g., title says "Smart-seq2" but abstract says "10x"), assign the platform from the title (higher semantic prominence) and note the conflict in the `notes` field.

---

## Modality Inference

Infer the sequencing modality from title and abstract text when no explicit modality field is available.

| Signal Words | Inferred Modality |
|---|---|
| "single-nucleus", "snRNA", "nuclei", "nucleus isolation" | `snRNA-seq` |
| "single-cell", "scRNA", "PBMC", "dissociated", "cell suspension" | `scRNA-seq` |
| "multiome", "ATAC+RNA", "joint profiling", "10x Multiome", "paired RNA and ATAC" | `multiome` |
| No modality signal found | `Unknown` |

### Modality Conflict Resolution

- If both "single-nucleus" and "single-cell" signals are present, prefer `snRNA-seq` (nucleus isolation is the more specific experimental distinction).
- If "multiome" signals are present alongside either single-cell or single-nucleus signals, use `multiome`.

---

## Platform Labels Reference

The following are the canonical platform label strings used throughout this skill:

| Label | Description |
|---|---|
| `10X Chromium` | 10x Genomics Chromium droplet-based platform (all versions) |
| `Smart-seq2` | Full-length transcript sequencing (plate-based) |
| `Drop-seq` | Droplet-based microfluidics (Macosko et al.) |
| `inDrops` | Droplet-based microfluidics (Klein et al.) |
| `MARS-seq` | Massively parallel RNA single-cell sequencing |
| `sci-RNA-seq` | Single-cell combinatorial indexing RNA-seq |
| `Microwell-seq` | Microwell-based high-throughput single-cell sequencing |
| `Unknown` | Platform could not be determined from available metadata |
