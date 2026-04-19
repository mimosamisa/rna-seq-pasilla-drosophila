# Methods

All analyses were performed on the Galaxy platform.

## Pipeline Overview

| Step | Tool | Version | Purpose |
|------|------|---------|---------|
| 1 | Falco + MultiQC | 1.0 / 1.27 | Raw read quality control |
| 2 | Cutadapt | 4.x | Adapter and quality trimming |
| 3 | RNA STAR | 2.7.11b+galaxy0 | Spliced read mapping to dm6 |
| 4 | JBrowse2 / pyGenomeTracks | — | Genome browser visualization |
| 5 | RSeQC Infer Experiment | — | Library strandedness detection |
| 6 | featureCounts | — | Gene-level read counting |
| 7 | DESeq2 | 1.x | Differential expression analysis |
| 8 | goseq | — | GO enrichment (Wallenius method) |
| 9 | Pathview | — | KEGG pathway visualization |

## Reference Genome
- **Genome:** UCSC dm6 (*Drosophila melanogaster* BDGP Release 6)
- **Annotation:** Ensembl BDGP6.32.109 (adapted for dm6 UCSC chromosome names)

## Key Parameters

### Cutadapt
- Quality cutoff: Q20 (3′ end)
- Minimum read length after trimming: 20 bp

### RNA STAR
- Reads: Paired-end (as collection)
- Reference: dm6 Full (built-in index)
- Gene model: Ensembl BDGP6.32.109_UCSC.gtf.gz
- Junction overhang: 36 bp (read length − 1)
- Output: Per gene read counts (GeneCounts)

### featureCounts
- Mode: Paired-end, Unstranded (confirmed by RSeQC)
- Feature type: exon
- Attribute type: gene_id

### DESeq2
- Design formula: ~LibraryType + Treatment
- Reference level: untreated
- Significance threshold: padj < 0.05
- |log2FC| threshold for GO/KEGG: > 1
