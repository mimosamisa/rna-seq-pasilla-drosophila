# 05 — Strandedness

## Tool
RSeQC — Infer Experiment (Galaxy)

## Description
Library strandedness was determined by comparing the alignment orientation
of mapped reads against annotated gene directions using a BED12 gene model
converted from the dm6 GTF annotation (41,200 regions).

## Results

| Sample | Failed to determine | Strand 1 fraction | Strand 2 fraction | Conclusion |
|--------|--------------------|--------------------|-------------------|------------|
| GSM461177 (untreated) | 3.29% | 48.55% | 48.16% | **Unstranded** |
| GSM461180 (treated) | 3.21% | 49.67% | 47.12% | **Unstranded** |

## Interpretation
An unstranded library shows approximately 50% of reads mapping to each
orientation. Both samples confirm the libraries are **unstranded**.
This result set `--strandedness 0` in featureCounts and DESeq2.

## Raw Output

### GSM461177
```
This is PairEnd Data
Fraction of reads failed to determine: 0.0329
Fraction of reads explained by "1++,1--,2+-,2-+": 0.4855
Fraction of reads explained by "1+-,1-+,2++,2--": 0.4816
```

### GSM461180
```
This is PairEnd Data
Fraction of reads failed to determine: 0.0321
Fraction of reads explained by "1++,1--,2+-,2-+": 0.4967
Fraction of reads explained by "1+-,1-+,2++,2--": 0.4712
```
