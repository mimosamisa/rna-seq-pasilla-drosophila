# 07 — PCA and Sample Distances

## Tool
DESeq2 v1.x (Galaxy) — built-in PCA and distance heatmap plots

## Description
Principal Component Analysis (PCA) and sample-to-sample distance heatmaps
were generated from DESeq2 variance-stabilized normalized counts across
all seven samples to assess replicate reproducibility and condition separation.

## Samples in This Analysis (all 7)
| Sample | Condition | Library Type |
|--------|-----------|-------------|
| GSM461176 | Untreated | Single-end |
| GSM461177 | Untreated | Paired-end |
| GSM461178 | Untreated | Paired-end |
| GSM461179 | Treated | Single-end |
| GSM461180 | Treated | Paired-end |
| GSM461181 | Treated | Paired-end |
| GSM461182 | Untreated | Single-end |

## PCA Results
| Component | Variance Explained | Biological Meaning |
|-----------|-------------------|-------------------|
| PC1 | **48%** | Treatment effect (treated vs untreated) |
| PC2 | **33%** | Library type (paired-end vs single-end) |

## Key Findings
- PC1 cleanly separates treated from untreated samples → strong PS depletion effect
- PC2 separates paired-end from single-end libraries → justifies multi-factor DESeq2 design
- Within-condition replicates cluster tightly → good reproducibility
- Sample distance heatmap confirms treated and untreated clusters are well separated


