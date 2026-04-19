# Results

Click any subfolder below to see figures, tables, and interpretation for that step.

| Folder | Step | Key Output |
|--------|------|-----------|
| [01_quality_control](01_quality_control/) | Raw read QC (Falco + MultiQC) | All files pass major metrics |
| [02_read_trimming](02_read_trimming/) | Cutadapt trimming | 91–99% reads retained |
| [03_read_mapping](03_read_mapping/) | STAR alignment to dm6 | 79–83% uniquely mapped |
| [04_strandedness](05_strandedness/) | RSeQC Infer Experiment | Libraries confirmed unstranded |
| [05_gene_counting](06_gene_counting/) | featureCounts | ~63% reads assigned |
| [06_pca_sample_distances](07_pca_sample_distances/) | DESeq2 PCA + heatmap | Treatment = PC1 (48% variance) |
| [07_differential_expression](08_differential_expression/) | DESeq2 | 967 significant genes |
| [08_go_enrichment](09_go_enrichment/) | goseq GO analysis | Glycogen metabolism, stress response |
| [9_kegg_pathway_analysis](10_kegg_pathway_analysis/) | goseq + Pathview | Glycolysis, Spliceosome pathways |
