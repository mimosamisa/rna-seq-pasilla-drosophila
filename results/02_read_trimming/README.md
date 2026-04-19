# 02 — Read Trimming

## Tool
Cutadapt v4.x (Galaxy wrapper)

## Description
Low-quality bases were trimmed from the 3′ end of reads (Q < 20) and
read pairs shorter than 20 bp after trimming were discarded.

## Results

| Sample | Input Pairs | Passing Pairs | Removed | Bases Trimmed |
|--------|------------|--------------|---------|---------------|
| GSM461177 (untreated) | 1,057,657 | 1,042,655 | 15,002 (1.4%) | 2.5% |
| GSM461180 (treated) | 1,226,483 | 1,116,426 | 110,057 (9.0%) | 12.4% |

## Key Findings
- GSM461177 retained 98.6% of read pairs — minimal trimming needed
- GSM461180 retained 91.0% — heavier R2 trimming (5.17M bp removed from R2)
- Both samples retained sufficient depth for downstream mapping
- The treated sample's higher trimming rate reflects lower R2 quality
  seen in the raw QC step
