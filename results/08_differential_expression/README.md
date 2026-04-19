# 08 — Differential Expression (DESeq2)

## Tool
DESeq2 v1.x (Galaxy wrapper)

## Description
DESeq2 was run with a multi-factor model accounting for both treatment
(treated vs untreated) and library type (paired-end vs single-end).
All seven samples were included. Genes with padj < 0.05 were considered
significantly differentially expressed.

## Model
```
design = ~ LibraryType + Treatment
reference level = untreated
```

## Summary Statistics
| Metric | Value |
|--------|-------|
| Total genes tested | 23,932 |
| Significant genes (padj < 0.05) | **967** |
| Significant with \|log2FC\| > 1 | subset used for GO/KEGG |
| Top downregulated gene | *SPARC* (log2FC = −2.38) |
| Top upregulated gene | *Ant2* (log2FC = +2.68) |

## Top 10 Most Significant Genes

| Gene Symbol | log2(FC) | Adj. p-value | Direction |
|-------------|----------|--------------|-----------|
| SPARC | −2.38 | 4.1 × 10⁻¹⁷⁵ | ↓ Treated |
| Kal1 | −4.08 | 2.8 × 10⁻¹⁷¹ | ↓ Treated |
| sesB | −2.99 | 2.8 × 10⁻¹⁷¹ | ↓ Treated |
| Ant2 | +2.68 | 1.9 × 10⁻¹⁵⁷ | ↑ Treated |
| Hml | −2.11 | 1.9 × 10⁻¹¹⁵ | ↓ Treated |
| CG1544 | −3.37 | 1.4 × 10⁻⁸⁴ | ↓ Treated |
| CG3770 | −2.42 | 1.7 × 10⁻⁸⁴ | ↓ Treated |
| lncRNA:CR43883 | −2.31 | 1.4 × 10⁻⁶⁶ | ↓ Treated |
| gas | −2.93 | 1.5 × 10⁻⁶² | ↓ Treated |
| Ama | +2.34 | 3.4 × 10⁻⁵⁶ | ↑ Treated |

> Note: *pasilla* itself (FBgn0261552, *ps*) is significantly downregulated
> (log2FC = −1.62, padj = 1.5 × 10⁻²⁹), confirming successful RNAi knockdown.

