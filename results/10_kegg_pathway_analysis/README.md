# 10 — KEGG Pathway Analysis

## Tools
- goseq (Galaxy) — KEGG pathway enrichment
- Pathview (Galaxy) — Pathway visualization

## Description
KEGG pathway enrichment was performed using goseq on the same set of
967 significantly DE genes. Two pathways were selected for Pathview
visualization based on enrichment significance and biological relevance.

## Enriched Pathways

| KEGG ID | Pathway Name | Significance |
|---------|-------------|-------------|
| dme00010 | Glycolysis / Gluconeogenesis | Significant |
| dme03040 | Spliceosome | Significant |

## Key Findings

### dme00010 — Glycolysis / Gluconeogenesis
- Multiple glycolytic and gluconeogenic enzymes are differentially expressed
- Red nodes = upregulated in treated cells
- Green nodes = downregulated in treated cells
- Consistent with GO enrichment showing glycogen/glucan biosynthesis changes
- Suggests PS depletion causes broad metabolic reprogramming in energy pathways

### dme03040 — Spliceosome
- Spliceosome component genes are differentially expressed after PS depletion
- *Pasilla* is itself a splicing factor (NOVA1/NOVA2 ortholog)
- Changes in spliceosome components suggest **compensatory or feedback
  regulation** of RNA splicing machinery when a key splicing factor is lost
- This is a biologically meaningful finding consistent with the known
  function of the *pasilla* gene
