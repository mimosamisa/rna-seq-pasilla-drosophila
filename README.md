# Reference-Based RNA-seq Analysis: Effect of Pasilla Gene Depletion in *Drosophila melanogaster*

**Platform:** Galaxy  
**Organism:** *Drosophila melanogaster* (dm6)  
**Condition:** RNAi-mediated depletion of the *pasilla* (PS) splicing factor  

---

## Table of Contents

1. [Introduction](#introduction)
2. [Methods](#methods)
3. [Results](#results)
   - [Quality Control](#1-quality-control)
   - [Read Trimming](#2-read-trimming)
   - [Read Mapping](#3-read-mapping)
   - [Genome Visualization](#4-genome-visualization)
   - [Strandedness](#5-strandedness)
   - [Gene Counting](#6-gene-counting-featurecounts)
   - [PCA and Sample Distances](#7-pca-and-sample-distance-heatmap)
   - [Differential Expression](#8-differential-expression-deseq2)
   - [GO Enrichment](#9-go-enrichment-analysis)
   - [KEGG Pathway Analysis](#10-kegg-pathway-analysis)
4. [Discussion](#discussion)
5. [Conclusion](#conclusion)
6. [References](#references)

---

## Introduction

The *pasilla* (*ps*) gene encodes a nuclear RNA-binding protein in *Drosophila melanogaster* that is the ortholog of mammalian NOVA1 and NOVA2, well-known neuronal splicing regulators. Understanding the transcriptomic consequences of *pasilla* depletion is important for modeling splicing regulation and RNA processing in eukaryotes.

This analysis follows the Galaxy Training Network reference-based RNA-seq tutorial (Brooks et al. 2011) to identify genes that are differentially expressed between PS-depleted (*treated*) and control (*untreated*) *Drosophila* cells. The workflow covers the complete analysis pipeline: raw read quality control, adapter trimming, genome mapping, gene counting, normalization, differential expression testing, and functional enrichment.

Two subsampled paired-end samples were used as the primary mapping demonstration:
- **GSM461177** — untreated, paired-end (~1.04M reads)
- **GSM461180** — PS-depleted (treated), paired-end (~1.12M reads)

For the differential expression analysis, all seven published samples from the Brooks et al. 2011 dataset were used (GSM461176–GSM461182), spanning both single-end and paired-end libraries from treated and untreated conditions.

---

## Methods

| Step | Tool | Version | Key Parameters |
|------|------|---------|----------------|
| Quality control | Falco + MultiQC | 1.0 / 1.27 | Default settings |
| Adapter trimming | Cutadapt | 4.x | Quality cutoff Q20, min length 20 bp |
| Genome mapping | STAR (RNA STAR) | 2.7.11b | dm6, Ensembl GTF, junction overhang 36 |
| Visualization | JBrowse2 / pyGenomeTracks | — | BAM coverage + gene annotations |
| Strandedness | RSeQC Infer Experiment | — | BED12 gene model from dm6 GTF |
| Gene counting | featureCounts | — | Paired-end, unstranded |
| Differential expression | DESeq2 | 1.x | ~Treatment + LibraryType |
| GO enrichment | goseq | — | Wallenius method, gene length bias correction |
| Pathway analysis | KEGG / Pathview | — | dme00010, dme03040 |

**Reference genome:** *Drosophila melanogaster* dm6  
**Gene annotation:** Ensembl BDGP6.32.109 (adapted for dm6 UCSC chromosome names)

---

## Results

### 1. Quality Control

Raw reads were assessed using Falco and summarized with MultiQC. All files showed acceptable quality for RNA-seq analysis.

| Metric | GSM461177 (untreated) | GSM461180 (treated) |
|--------|----------------------|---------------------|
| Read length | 37 bp | 37 bp |
| Total reads | 1,057,657 | 1,226,483 |
| %GC | 55% | 56% |
| Per base quality (R1) | PASS | PASS |
| Per base quality (R2) | WARN (3′ drop) | WARN (3′ drop) |
| Adapter content | PASS | PASS |
| Per base N content | PASS | PASS |
| Sequence duplication | PASS | PASS |
| Per base sequence content | FAIL* | FAIL* |
| Per tile sequence quality | FAIL† | FAIL† |

*FAIL for per base sequence content is expected in RNA-seq due to random priming biases at read starts.  
†FAIL for per tile quality reflects flow cell tile-level variation; normal in Illumina data.

**Key observation:** The 3′ quality drop in reverse reads (R2) is typical of Illumina paired-end sequencing and is corrected by quality trimming in the next step. No adapter contamination was detected above 0.1%, indicating library preparation was clean.

---

### 2. Read Trimming

Cutadapt was applied to remove low-quality bases (Q < 20) and discard reads shorter than 20 bp after trimming.

| Sample | Input Pairs | Passing Pairs | Removed | Bases Trimmed |
|--------|------------|--------------|---------|---------------|
| GSM461177 (untreated) | 1,057,657 | 1,042,655 | 15,002 (1.4%) | 2.5% |
| GSM461180 (treated) | 1,226,483 | 1,116,426 | 110,057 (9.0%) | 12.4% |

The treated sample had a higher proportion of reads removed (9.0% vs 1.4%), largely due to more aggressive base quality trimming in the reverse read (R2), which had 5,169,801 bp trimmed vs 873,051 bp in the untreated sample. This is consistent with the slightly lower average read quality seen in QC. Both samples retained sufficient read depth for downstream mapping.

---

### 3. Read Mapping

Trimmed reads were mapped to the *Drosophila melanogaster* dm6 genome using STAR with splice-junction-aware alignment. The Ensembl GTF annotation was used to guide junction detection, with an overhang parameter of 36 (read length − 1).

| Metric | GSM461177 (untreated) | GSM461180 (treated) |
|--------|----------------------|---------------------|
| Input reads | 1,042,655 | 1,116,426 |
| Uniquely mapped | 866,834 (83.1%) | 881,782 (79.0%) |
| Multi-mapped (multiple loci) | 57,393 (5.5%) | 50,684 (4.5%) |
| Mapped to too many loci | 56,924 (5.5%) | 59,082 (5.3%) |
| Unmapped (too short) | 59,821 (5.7%) | 124,066 (11.1%) |
| Mismatch rate | 0.77% | 1.73% |
| Annotated splices | 94,197 | 92,597 |

Both samples exceed the 70% unique mapping threshold used as a quality benchmark, confirming successful alignment to the reference genome. The higher unmapped rate in the treated sample (11.1% too short) reflects the greater amount of trimming applied. The low mismatch rates (<2%) indicate high alignment fidelity.

---

### 4. Genome Visualization

Aligned BAM files were visualized using JBrowse2 and pyGenomeTracks to confirm read distribution across gene bodies and splice junctions.
The coverage plots confirm that reads are distributed across exonic regions with clear gaps at introns, consistent with proper spliced alignment. No major mapping artifacts were observed.

---

### 5. Strandedness

Library strandedness was determined using RSeQC's Infer Experiment tool on both mapped samples.

| Sample | Failed to determine | Fraction "1++,1--" | Fraction "1+-,1-+" |
|--------|--------------------|--------------------|---------------------|
| GSM461177 (untreated) | 3.29% | 48.55% | 48.16% |
| GSM461180 (treated) | 3.21% | 49.67% | 47.12% |

Both samples show approximately equal fractions (~48–50%) mapping to each orientation, confirming that the libraries are **unstranded**. This result was used to set featureCounts and DESeq2 parameters accordingly.

---

### 6. Gene Counting (featureCounts)

Read counts per gene were obtained using featureCounts in paired-end, unstranded mode against the dm6 Ensembl GTF annotation.

| Metric | GSM461177 (untreated) | GSM461180 (treated) |
|--------|----------------------|---------------------|
| Assigned reads | 825,225 (63.0%) | 843,496 (~62%) |
| Unassigned: unmapped | 118,428 | 183,960 |
| Unassigned: multi-mapping | 324,121 | 273,323 |
| Unassigned: no features | 20,088 | 14,778 |
| Unassigned: ambiguity | 2,152 | 23,508 |

Approximately 63% of reads were successfully assigned to annotated features in both samples. The multi-mapping category accounts for the largest fraction of unassigned reads, consistent with the STAR multi-mapping rates seen above.

---

### 7. PCA and Sample Distance Heatmap

Principal Component Analysis (PCA) and sample-to-sample distance heatmaps were generated from DESeq2 normalized counts across all seven samples.

The PCA showed that the dominant source of variance (PC1, 48%) is the treatment effect (PS depletion), while sequencing type (paired-end vs single-end) drives PC2 (33%). This validates the multi-factor design used in DESeq2.

---

### 8. Differential Expression (DESeq2)

DESeq2 was run with a model accounting for both treatment (treated vs untreated) and library type (paired-end vs single-end). Genes with adjusted p-value < 0.05 were considered differentially expressed.

**Summary:**
- Total genes tested: 23,932
- Significantly DE genes (padj < 0.05): **967**
- Genes with padj < 0.05 and |log2FC| > 1: subset used for downstream analyses

**Top 10 Most Significantly Differentially Expressed Genes:**

| Gene ID | Gene Symbol | Base Mean | log2(FC) | Adj. p-value | Direction |
|---------|------------|-----------|----------|--------------|-----------|
| FBgn0026562 | SPARC | 43,869 | -2.38 | 4.1 × 10⁻¹⁷⁵ | ↓ Treated |
| FBgn0039155 | Kal1 | 736 | -4.08 | 2.8 × 10⁻¹⁷¹ | ↓ Treated |
| FBgn0003360 | sesB | 4,393 | -2.99 | 2.8 × 10⁻¹⁷¹ | ↓ Treated |
| FBgn0025111 | Ant2 | 1,508 | +2.68 | 1.9 × 10⁻¹⁵⁷ | ↑ Treated |
| FBgn0029167 | Hml | 3,664 | -2.11 | 1.9 × 10⁻¹¹⁵ | ↓ Treated |
| FBgn0039827 | CG1544 | 265 | -3.37 | 1.4 × 10⁻⁸⁴ | ↓ Treated |
| FBgn0035085 | CG3770 | 644 | -2.42 | 1.7 × 10⁻⁸⁴ | ↓ Treated |
| FBgn0264475 | lncRNA:CR43883 | 651 | -2.31 | 1.4 × 10⁻⁶⁶ | ↓ Treated |
| FBgn0034736 | gas | 222 | -2.93 | 1.5 × 10⁻⁶² | ↓ Treated |
| FBgn0000071 | Ama | 322 | +2.34 | 3.4 × 10⁻⁵⁶ | ↑ Treated |

The most downregulated gene is *SPARC* (Secreted Protein Acidic and Rich in Cysteine), a conserved extracellular matrix glycoprotein. The most upregulated gene is *Ant2* (Adenine nucleotide translocase 2), involved in mitochondrial ADP/ATP exchange. Notably, the *pasilla* gene itself (FBgn0261552, *ps*) is significantly downregulated (log2FC = −1.62, padj = 1.5 × 10⁻²⁹), confirming effective RNAi knockdown.

---

### 9. GO Enrichment Analysis

Gene Ontology (GO) enrichment was performed using goseq with the Wallenius method to correct for gene-length bias. The 967 significantly DE genes were tested for over-representation across all GO categories (CC = Cellular Component, BP = Biological Process, MF = Molecular Function).

**Top Over-Represented GO Categories:**

| GO Category | Term | Adj. p-value | % DE in category |
|-------------|------|-------------|-----------------|
| BP | Glycogen biosynthetic process | < 0.001 | ~80% |
| BP | Glucan biosynthetic process | < 0.001 | ~80% |
| BP | Septate junction assembly | < 0.001 | ~37% |
| BP | Establishment of blood-brain barrier | ~0.001 | ~50% |
| BP | Small molecule metabolic process | ~0.002 | ~12% |
| BP | Response to stress | ~0.002 | ~12% |
| MF | Oxidoreductase activity | ~0.002 | ~12% |
| MF | Transferase activity (alkyl groups) | ~0.003 | ~22% |
| MF | Glutathione transferase activity | ~0.003 | ~22% |
| CC | Extracellular region | ~0.001 | ~12% |

The enrichment of glycogen metabolism terms is notable, as *pasilla*/NOVA splicing factors regulate metabolic gene expression post-transcriptionally. The enrichment of extracellular region and blood-brain barrier terms reflects changes in *SPARC* and other secreted/structural proteins.

---

### 10. KEGG Pathway Analysis

KEGG pathway analysis was performed using goseq and visualized with Pathview for selected pathways. Two pathways showed significant enrichment.

**dme00010 — Glycolysis / Gluconeogenesis**

**dme03040 — Spliceosome**

---

## Discussion

This analysis successfully reproduced the key findings of the Brooks et al. (2011) study using a streamlined Galaxy-based pipeline on subsampled data.

**Quality and Mapping:** Both samples achieved acceptable unique mapping rates (79–83%), well above the 70% quality benchmark. The greater trimming and higher unmapped fraction in the treated sample (GSM461180) did not compromise the overall analysis, as sufficient read depth was retained.

**Strandedness:** Both samples were confirmed to be unstranded, with approximately equal read fractions mapping to each strand (~48–50%). This is consistent with the dUTP-free library preparation method used in the original study and correctly informed downstream featureCounts and DESeq2 settings.

**Differential Expression:** The multi-factor DESeq2 model (accounting for sequencing type) identified 967 significantly DE genes. The PCA confirmed that treatment is the dominant source of variance, validating the experimental design. The *pasilla* gene itself was among the DE genes, confirming effective RNAi knockdown.

The most significantly downregulated gene, *SPARC*, is an extracellular matrix protein linked to cell-matrix interactions and Drosophila hemocyte function. Its strong downregulation upon PS depletion suggests that PS regulates *SPARC* mRNA stability or splicing. Upregulation of *Ant2* (mitochondrial ADP/ATP translocase) may reflect metabolic compensation.

**Functional Enrichment:** GO and KEGG analyses identified enrichment in glycogen/glucan metabolism, which is consistent with the known role of NOVA/pasilla-family proteins in regulating metabolic mRNA processing. The spliceosome pathway enrichment in KEGG suggests that loss of a key splicing factor (*pasilla*) may trigger compensatory changes in the expression of other splicing components — a form of RNA-level homeostasis.

**Limitations:** The analysis used subsampled files (~1M reads) rather than the full dataset, which may reduce power to detect low-abundance transcripts. Additionally, only two samples were used for the mapping step, while all seven were used for DESeq2, creating some inconsistency in read depth across the dataset. Future work should incorporate full-depth sequencing and splicing-level analysis (e.g., rMATS) to capture the primary regulatory role of PS as a splicing factor.

---

## Conclusion

This reference-based RNA-seq pipeline successfully identified transcriptomic changes caused by *pasilla* depletion in *Drosophila melanogaster*. Key findings include:

- Both samples showed high-quality mapping (>79% unique) to dm6 after quality trimming
- Libraries were confirmed to be unstranded by RSeQC
- 967 genes were significantly differentially expressed (padj < 0.05)
- Top downregulated genes include *SPARC*, *sesB*, *Kal1*, and *Hml*
- Top upregulated genes include *Ant2* and *Ama*
- The *pasilla* gene itself was significantly downregulated, confirming successful RNAi
- GO enrichment highlighted glycogen metabolism and extracellular region functions
- KEGG analysis identified glycolysis and spliceosome pathway changes

The results are consistent with published literature and demonstrate the utility of Galaxy as a platform for reproducible RNA-seq analysis.

---

## References

1. Brooks, A.N. et al. (2011). Conservation of an RNA regulatory map between Drosophila and mammals. *Genome Research*, 21(2), 193–202. https://doi.org/10.1101/gr.108662.110

2. Dobin, A. et al. (2013). STAR: ultrafast universal RNA-seq aligner. *Bioinformatics*, 29(1), 15–21. https://doi.org/10.1093/bioinformatics/bts635

3. Love, M.I., Huber, W. & Anders, S. (2014). Moderated estimation of fold change and dispersion for RNA-seq data with DESeq2. *Genome Biology*, 15, 550. https://doi.org/10.1186/s13059-014-0550-8

4. Liao, Y., Smyth, G.K. & Shi, W. (2014). featureCounts: an efficient general purpose program for assigning sequence reads to genomic features. *Bioinformatics*, 30(7), 923–930. https://doi.org/10.1093/bioinformatics/btt656

5. Wang, L., Wang, S. & Li, W. (2012). RSeQC: quality control of RNA-seq experiments. *Bioinformatics*, 28(16), 2184–2185. https://doi.org/10.1093/bioinformatics/bts356

6. Young, M.D. et al. (2010). Gene ontology analysis for RNA-seq: accounting for selection bias. *Genome Biology*, 11, R14. https://doi.org/10.1186/gb-2010-11-2-r14

7. Martin, M. (2011). Cutadapt removes adapter sequences from high-throughput sequencing reads. *EMBnet Journal*, 17(1), 10–12. https://doi.org/10.14806/ej.17.1.200

8. Ewels, P. et al. (2016). MultiQC: summarize analysis results for multiple tools and samples in a single report. *Bioinformatics*, 32(19), 3047–3048. https://doi.org/10.1093/bioinformatics/btw354

9. Galaxy Training Network. Reference-based RNA-seq data analysis. https://training.galaxyproject.org/training-material/topics/transcriptomics/tutorials/ref-based/tutorial.html

---



*This analysis was performed as part of an RNA-seq bioinformatics course assignment using the Galaxy platform.*
