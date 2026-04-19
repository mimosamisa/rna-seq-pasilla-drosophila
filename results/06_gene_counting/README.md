# 06 — Gene Counting (featureCounts)

## Tool
featureCounts (Subread package, Galaxy wrapper)

## Description
Read counts per gene were obtained using featureCounts in paired-end,
unstranded mode against the dm6 Ensembl BDGP6.32.109 GTF annotation.
23,933 genes were quantified.

## Results

| Metric | GSM461177 (untreated) | GSM461180 (treated) |
|--------|----------------------|---------------------|
| Assigned reads | 825,225 **(63.0%)** | 843,496 (~62%) |
| Unassigned: unmapped | 118,428 | 183,960 |
| Unassigned: multi-mapping | 324,121 | 273,323 |
| Unassigned: no features | 20,088 | 14,778 |
| Unassigned: ambiguity | 2,152 | 23,508 |

## Key Findings
- ~63% of reads successfully assigned to annotated features in both samples
- Multi-mapping is the largest source of unassigned reads (consistent with STAR output)
- Low no-feature rate confirms good annotation coverage of the dm6 genome
- Assignment rates are acceptable for downstream DESeq2 analysis
