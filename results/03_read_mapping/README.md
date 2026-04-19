# 03 — Read Mapping

## Tool
RNA STAR v2.7.11b+galaxy0

## Description
Trimmed reads were mapped to the *Drosophila melanogaster* dm6 genome using
STAR with splice-junction-aware alignment. The Ensembl GTF annotation was
used to guide junction detection (overhang = 36 bp = read length − 1).

## Results

| Metric | GSM461177 (untreated) | GSM461180 (treated) |
|--------|----------------------|---------------------|
| Input reads | 1,042,655 | 1,116,426 |
| Uniquely mapped | 866,834 **(83.1%)** | 881,782 **(79.0%)** |
| Multi-mapped (multiple loci) | 57,393 (5.5%) | 50,684 (4.5%) |
| Mapped to too many loci | 56,924 (5.5%) | 59,082 (5.3%) |
| Unmapped (too short) | 59,821 (5.7%) | 124,066 (11.1%) |
| Mismatch rate | 0.77% | 1.73% |
| Annotated splices | 94,197 | 92,597 |

## Key Findings
- Both samples exceed the 70% unique mapping quality threshold ✅
- Low mismatch rates (< 2%) confirm high alignment fidelity
- Higher unmapped rate in treated sample reflects heavier trimming
- Annotated splice junctions dominate (GT/AG canonical: > 99%)
