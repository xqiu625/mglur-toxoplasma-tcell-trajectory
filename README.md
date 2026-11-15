# mGluR T-cell trajectories in chronic Toxoplasma infection

R scripts for single-cell trajectory and pseudotime analysis associated with the study by **Vizcarra et al. (2023)** on group 1 metabotropic glutamate receptors (mGluRs) and memory CD8+ T cells during chronic *Toxoplasma gondii* infection in the central nervous system.

The repository contains the two original **Seurat / Monocle3** analysis scripts. It covers their trajectory-analysis component; input data, generated figures, and a complete reproduction of the paper are not included.

## Related publication

Vizcarra EA, Ulu A, Landrith TA, Qiu X, Godzik A, Wilson EH. **Group 1 metabotropic glutamate receptor expression defines a T cell memory population during chronic Toxoplasma infection that enhances IFN-gamma and perforin production in the CNS.** *Brain, Behavior, and Immunity*. 2023;114:131–143.

[DOI: 10.1016/j.bbi.2023.08.015](https://doi.org/10.1016/j.bbi.2023.08.015) · [PubMed: 37604212](https://pubmed.ncbi.nlm.nih.gov/37604212/) · [ScienceDirect](https://www.sciencedirect.com/science/article/pii/S0889159123002398)

The study examines how mGluR expression relates to the memory phenotype and effector functions of CD8+ T cells in the infected brain. These scripts explore cell-state relationships using a learned trajectory graph and pseudotime.

## Repository contents

| Script | Analysis implemented |
| --- | --- |
| [Fig2A_trajectory.R](Fig2A_trajectory.R) | Loads the `Fig2A` count matrix and `LibraryID.csv`, derives `mGluR-` / `mGluR+` labels from library IDs, aligns cells by `LibraryID`, and plots trajectories and pseudotime. |
| [Fig4A_trajectory.R](Fig4A_trajectory.R) | Loads the `Fig4A` count matrix and cluster annotations, retains clusters 1–3, derives sample IDs from barcode suffixes, aligns cells by `Sample.ID`, and plots trajectories and pseudotime. |

The `Fig2A` and `Fig4A` names are retained from the original scripts and input folders. The `Fig2A` script stores the mGluR expression labels in a metadata column named `Genotype`.

Both scripts use 100 preprocessing dimensions, UMAP, cell clustering, graph learning, and cell ordering in Monocle3. The selected root principal nodes are `Y_1` for `Fig2A` and `Y_3` for `Fig4A`.

## Required inputs

Provide the original 10x filtered feature-barcode matrices and matching cell annotations:

```text
Fig2A_Outs_Folder/
├── filtered_feature_bc_matrix/
└── LibraryID.csv

Fig4A_Outs_Folder/
├── filtered_feature_bc_matrix/
└── K-Means 4.csv
```

`LibraryID.csv` must contain `Barcode` and `LibraryID` columns. The second annotation file must contain `Barcode` and a cluster column read by R as `X4`, which the script renames to `Cluster`. The retained cluster labels are `Cluster 1`, `Cluster 2`, and `Cluster 3`; barcode suffixes `1`, `2`, and `3` identify samples in this script.

## Running the scripts

1. Use an R environment with the packages loaded by the scripts: `SingleCellExperiment`, `Seurat`, `tidyverse`, `Matrix`, `scales`, `cowplot`, `RCurl`, `stringr`, `dplyr`, `plyr`, and `monocle3`. Original package versions are not recorded in this repository.
2. Update the absolute input paths, `.libPaths()` setting, and output directory in each script for your environment. The saved paths refer to the original research computing environment.
3. Create the output directory and verify that the metadata match the matrix barcodes. Confirm the root-node choice against the learned graph before interpreting pseudotime, especially when changing the data or package versions.
4. Run the scripts from R or with `Rscript` after configuring those paths:

```bash
Rscript Fig2A_trajectory.R
Rscript Fig4A_trajectory.R
```

Each script writes three PNG plots to the configured `Monocle3_output` directory: two trajectory views colored by mGluR label or cluster, and one view colored by pseudotime. Output filenames follow `Monocle3_Traject_2A_v1.png` through `v3.png`, or the corresponding `4A` names.

## Citation

Please cite the publication above when using these study-associated analyses, and cite Seurat and Monocle3 as appropriate for the methods used.

This repository was previously named `302011_git`.
