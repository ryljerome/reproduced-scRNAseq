# reproduced-scRNAseq
### Project 2 for BF550 ML:

The goal of the project is to practice data analysis in Python. To that end, each student should choose a paper and reproduce at least two of its figures. 
The idea is to use data analytics methods (ML methods, regressions etc.), not simply plot the data. You need to find a paper where the authors use data analytics methods, and reproduce couple of the figures in it.

scRNA-seq uncovers the transcriptional dynamics of Encephalitozoon intestinalis parasites in human macrophages
Jaroenlak, P., McCarty, K.L., Xia, B. et al. scRNA-seq uncovers the transcriptional dynamics of Encephalitozoon intestinalis parasites in human macrophages. Nat Commun 16, 3269 (2025). https://doi.org/10.1038/s41467-025-57837-z

# Project Code Outline

### Figures of Interest to Replicate

Figure 2: UMAPs by donor
- 4 UMAPs showing total transcriptome clustering by donor

Figure 5: Pathway Analysis of specific cluster
- cluster is identified from comparing two host-only clustering UMAPs
- UMAPs are uninfected cells and all cells

### Steps to Prepare All Data:
- read in the data per donor
- demultiplex the donor data then remove doublets and negatives; AND separate the combined donor 3 and donor 4 data
- QC control for all donor data -- results in d#_adata_qc, 4 AnnData Objects
- add important columns to each of these AnnData objects for future analysis (species and pct_parasite)
- datasets separated by donor = d#_adata_qc used for subsetting and these are used for figure 2 specifc UMAPs --> qc_data = list(d1_adata_qc, d2_adata_qc, d3_adata_qc, d4_adata_qc)
- create subsets for figure 5: extract host-only transctips from each donor by species column
- leaves datasets of host_only = list(d1_human, d2_human, d3_human, d4_human)

### Steps to Prepare Data for Figure 2 Plotting:
- data for fig 2: list(d1_adata_qc, d2_adata_qc, d3_adata_qc, d4_adata_qc)
- normalize
- unique_index
- find HVGs
- create list of HVGs
- subset each of the 4 AnnData objects to only include the HVGs
- integrate all 4 into 1 (integrate and then concat)
- scale this 1 AnnData object

### Steps to Prepare Data for Figure 5 Plotting:
- data for fig 5: list(d1_human, d2_human, d3_human, d4_human)
- normalize
- unique_index
- find HVGs
- create list of HVGs
- subset each of the 4 AnnData objects to only include the HVGs
- integrate all 4 into 1 (integrate and then concat)
- scale this 1 AnnData object

### PCA then UMAP Visualization:
- for each scaled and integrated AnnData object for Fig 2 or Fig 5......
- calculate PCA, neighbors, UMAP, and cluster with leiden on the scaled, integrated data
- maintain number of PCs and resolution that was used in the paper
- fig 2 uses 1-41 PCs at a resolution of 0.4 to get 12 clusters
- then for the fig 2, the integrated anndata can be separated by donor once again
- processing of the host-only transcripts ises 1-43 PCs at a resolution of 0.2 to get 9 clusters
- the host_transcript only data was subset right before plotting to obtain a control dataset that was only cells that were considered uninfected.
- run sc.pl.umap

### Enrichment Analysis:
- Identify cluster from figure 5 umaps showing the most difference
- isolate the cells and their genes, filtering for FC > 1.5
- run EnrichR using Molecular Signatures Hallmark 2020
- make bar chart ranked by p-vales

