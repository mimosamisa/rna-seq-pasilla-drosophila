# 09 — GO Enrichment Analysis

## Tool
goseq (Galaxy wrapper) — Wallenius method

## Description
Gene Ontology (GO) enrichment was performed on the 967 significantly
differentially expressed genes (padj < 0.05) using goseq. The Wallenius
method corrects for gene-length bias, which is important because longer
genes tend to have more reads and are therefore more likely to be detected
as differentially expressed.

Categories tested: Cellular Component (CC), Biological Process (BP),
and Molecular Function (MF).

## Top Over-Represented GO Categories

| GO Domain | Term | Adj. p-value | % DE in Category |
|-----------|------|-------------|-----------------|
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

## Key Findings
- **Glycogen/glucan biosynthesis** shows the highest DE gene fraction (~80%)
  suggesting major metabolic reprogramming upon PS depletion
- **Extracellular region** enrichment reflects downregulation of *SPARC*
  and other secreted/structural proteins
- **Response to stress** and **oxidoreductase activity** suggest cellular
  stress responses are activated in PS-depleted cells
- **Septate junction and blood-brain barrier** terms reflect changes in
  cell adhesion and barrier function genes
