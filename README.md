# Analysis of scRNA-Seq data from NF1 patients

The analysis described here is presented in the following publications:

> [Brunet *et al*]() (not yet accepted)

> [Radomska *et al*]() (not yet accepted)

To read the analysis, please open `index.html` (local machine) or open it using the following link:

https://audrey-onfroy.github.io/brunet_et_al/

## Detailed methods

### Data

The data investigated here are issued from 3 locations:

* our laboratory: check for the last authors in the publications (3 dyNF and 1 MPNST samples)
* [Wu *et al*, 2022, Science Advances](https://doi.org/10.1126/sciadv.abo5442) for the GSE179033 dataset (9 pNF and 4 MPNST samples)
* [Sun *et al*, 2019, Science Advances](https://doi.org/10.1016/j.stem.2021.04.029) for the GSE165826 dataset (1 dyNF sample)

### From FASTQ files from count matrices

The public datasets were retrieved at the stage of count matrices. Refer to the original publications for the processing of FASTQ files. Regarding our data, the FASTQ files were processed using Cell Ranger software from 10X Genomics. Briefly, BCL files were converted to fastq files, then reads were extracted based on sample index, and mapped onto a Homo sapiens transcriptome version hg19, using 10x Genomics Cell Ranger 3.1.0 mkfastq and count functions, with default settings.

### Processing of the count matrices

#### General tools

We used R Markdown to generate fully traceable files for each analysis. Compiled HTML files are accessible through the Github repository. A Singularity container containing R version 3.6.3 and all specific versions for all the packages used for the analyses was developed to ensure version stability. It is accessible on Zenodo with [record ID 13145084](https://zenodo.org/records/13145084). R Markdown files were developed within R Studio but compiled in the terminal using the Singularity container.

#### Count matrices to indiviual annotated objects

Each dataset was analyzed individually. Count matrices were loaded using Seurat V3 package. Following filtering steps were applied. Cells with a log number of UMI lower than 6 or expressing a number of genes lower than 600 were removed. Then, doublet cells were removed using scDblFinder tool and scds in hybrid mode. Finally, cells having more than 20 % of UMI related to mitochondrial genes or more than 30 % of UMI related to ribosomal genes were filtered out. UMI count matrix for remaining cells was normalized using LogNormalize method implemented in Seurat V3 package. From the normalized count matrix, AddModuleScore was used to annotated cells for cell type using the marker sets provided as supplementary table. Individual datasets were then saved for further analyses.

#### Combined dataset

##### Gene annotation homogenization

Each dataset was not mapped using the same reference. To homogenize the annotation, correspondences between gene name and gene Ensembl ID, for each dataset, was extracted from their respective count matrix file. From 27,955 to 63,677 genes available in each dataset, 25,696 commonly available genes, based on Ensembl ID, were kept. Gene names were homogenized based on the transcriptome version we used for our datasets, hg19. Gene filtering and renaming was applied on non-normalized count matrix for each individual dataset. All 18 datasets were combined in one Seurat objected. Genes expressed in less than 5 cells were removed, leading to 22,624 genes remaining.

##### Processing

Merged UMI count matrix was normalized using LogNormalize method implemented in Seurat V3 package. A PCA was generated using Seurat’s RunPCA function with 100 principal components (PC) from 2,000 highly variable features, identified using Seurat’s FindVariableFeatures function with default settings. Dataset-specific effect was removed on the PCA using RunHarmony function from harmony package with seed set and max.iter.harmony set to 50 (instead of 20) to ensure convergence. Some tumor cells were difficult to distinguish from fibroblasts. Thus, a large number of dimensions (n = 80) was used to generate a 2D projection. Both UMAP and tSNE were generated from 80 dimensions from harmony, using the implementation from Seurat package.


##### Cell type annotation

Cell type annotation was kept from the individual analyses. This annotation is made at a single-cell level. To remove eventual mis-annotation, the annotation was smoothed at a cluster level. To obtain a clustering, a graph using Seurat’s FindNeighbors function on 80 dimensions for the harmonized PCA, and then clustered cells from this graph using Seurat’s FindClusters function with a resolution of 1 was first generated. To smooth the annotation, we computed the proportion of cell type by clusters was computed and for each cluster the most abundant cell type was retained. For cell family annotation, cell family was associated to cell types and relabeled clusters.

### Figures

All figures are made using Seurat, ggplot2 and patchwork packages.

## References

Below are the references to the various tools we used :

- rmarkdown : Yihui Xie, J.J. Allaire and Garrett Grolemund (2018). R Markdown: The Definitive Guide. Chapman and Hall/CRC. ISBN 9781138359338. URL: https://bookdown.org/yihui/rmarkdown.=
- Singularity : https://doi.org/10.1371/journal.pone.0177459
- Seurat V3 : https://doi.org/10.1016/j.cell.2019.05.031
- scDblFinder : https://doi.org/10.12688/f1000research.73600.2
- scds : https://doi.org/10.1093/bioinformatics/btz698
- harmony : https://doi.org/10.1038/s41592-019-0619-0
- ggplot2 : Wickham H. (2016). ggplot2: Elegant Graphics for Data Analysis. Springer-Verlag New York. ISBN 978-3-319-24277-4, https://ggplot2.tidyverse.org
- patchwork : Pedersen T. (2022). patchwork: The Composer of Plots. https://patchwork.data-imaginist.com, https://github.com/thomasp85/patchwork

## Citation

To cite the data, please refer to their publication of origin.

To cite the R packages and other tools, please refer to original publications.

To cite this work (directory tree, modular files organisation, methological approach...), please use:

> 12 tips paper (not yet published)

<br><br>

| ![CC](https://upload.wikimedia.org/wikipedia/commons/d/d3/Cc_by-nc_icon.svg) | Except where otherwise noted, this work is licensed under <br> [https://creativecommons.org/licenses/by-nc/4.0/](https://creativecommons.org/licenses/by-nc/4.0/) |
|:----:|:---:|