# 01 — Quality Control

## Tool
Falco + MultiQC v1.27

## Description
Raw FASTQ reads were assessed for quality using Falco. Results from all four
files were aggregated into a single report using MultiQC.

## Samples
| Sample | Condition | Library | Total Reads | %GC |
|--------|-----------|---------|-------------|-----|
| GSM461177 | Untreated | Paired-end | 1,057,657 | 55% |
| GSM461180 | Treated (PS RNAi) | Paired-end | 1,226,483 | 56% |

## Key Findings
- Read length: 37 bp for all files
- Forward reads (R1): PASS on all major metrics
- Reverse reads (R2): WARN on per-base quality (expected 3′ drop)
- No adapter contamination detected (> 0.1%)
- Per base sequence content: FAIL (expected for RNA-seq random priming)
- Per tile sequence quality: FAIL (normal flow cell variation)


