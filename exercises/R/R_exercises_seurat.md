
# Exercises: Intro to Seurat


## Context

Imagine you’ve just received a sheep scRNA-seq dataset generated using SMART-seq2. The sequencing provider has already completed the basic bioinformatics steps such as alignment and quantification. Your PI has now handed you the raw counts matrix and asked you to analyse it.

In this workshop, we will use R and Seurat to take the dataset from raw counts through to normalisation. You will learn how to create a Seurat object, filter low-quality cells and features, and apply appropriate normalisation methods.

In a full scRNA-seq workflow, the next steps would include identifying variable features, scaling, dimensionality reduction, clustering, annotation, and downstream analyses such as differential expression. We won’t cover those here, the aim of this session is to build a solid foundation by focusing on the essential preprocessing steps required before any of the more advanced analyses can be performed.


## 1. Load Data 

You begin with a raw counts matrix produced by the sequencing provider.
Your first task is to load the matrix into R so that we can prepare it for downstream analysis. For the purpose of this exercise you are provided with the following synthetic data. Keep in mind real datasets are (i) bigger and (ii) can be slightly different formats. It's good practice to check it before you move on. 

```
# Please run the following code:
data <- read.csv(text = "
Gene,chr,strand,start,end,length,Cell1,Cell2,Cell3,Cell4,Cell5,Cell6,Cell7,Cell8
ACTB,chr7,+,55328000,55330500,2500,1380,1502,1298,1420,1355,1478,1399,1521
GAPDH,chr12,+,4025300,4027100,1800,1120,1185,1090,1202,1158,1210,1134,1198
RPL13,chr1,-,34589000,34590000,1100,460,512,441,498,470,520,455,510
RPS6,chr2,+,12845000,12846200,1200,350,368,322,360,341,375,330,362
HBB,chr15,-,2285000,2286200,1200,210,198,225,218,205,212,200,230
S100A10,chr4,+,81670000,81671200,1200,98,110,102,115,105,118,103,112
CXCL12,chr10,+,95032000,95032800,800,0,5,2,4,1,3,0,6
MMP19,chr3,-,68740000,68740900,900,42,55,38,52,46,57,40,53
MT-CO1,chrM,+,590,1540,950,70,82,61,78,67,80,69,84
MT-ND1,chrM,+,3300,4260,960,59,64,52,60,55,63,58,66
PTPRC,chr20,-,44710000,44712700,2700,15,22,10,18,12,20,13,19
CD74,chr5,+,15078000,15079500,1500,25,30,19,31,22,33,21,29
", row.names = 1)

# inspect the data
data

```

Tasks:

* Use `head()` and `dim()` to inspect the dataset.
* What data type and class are the counts (`typeof()` and `class()`)?
* What are the headers names?
* How many genes and how many cells are in the matrix?


```
# Your code here


```

## 2. Seurat object

Your dataset contains gene metadata in the first few columns (e.g., chromosome, start, end) and raw SMART-seq2 counts in the remaining columns. Before creating a Seurat object, you need to make sure that only the count columns are used.

A Seurat object keeps data in layers such as:
* counts - raw integer counts
* data - normalised values
* scale.data - scaled values (used for PCA)

`GetAssayData()` = Give me the raw counts matrix from the RNA assay inside this Seurat object.

For more info, go to the Seurat [documentation](https://satijalab.org/seurat/articles/essential_commands). 

Tasks:

* Look at the structure of your data frame using functions such as `head()`, `colnames()`, or `str()`.
* Identify which columns contain cell-level raw counts.
* Extract only those count columns.
* Use that subset to create a Seurat object - use `CreateSeuratObject()`, please see: https://www.rdocumentation.org/packages/Seurat/versions/3.0.1/topics/CreateSeuratObject and https://satijalab.org/seurat/articles/pbmc3k_tutorial.html.
* Explore your Seurat object to see how Seurat organises single-cell data - use the [documentation](https://satijalab.org/seurat/articles/essential_commands):
Find out:
(i) how many genes are in your dataset
(ii) how many cells were loaded
(iii) what components and layers make up the object’s internal structure

```
# if it's not installed:
# install.packages("Seurat")

# load package
library(Seurat)

# Your code here:

```

## 3. Filter data
Before analysing single-cell data, we must remove low-quality cells that can distort downstream results. SMART-seq2 datasets often contain a mixture of:
* healthy cells
* stressed or dying cells
* empty wells (very low counts)
* doublets (two cells sequenced together)

To distinguish these, we rely on three key QC metrics automatically stored in the Seurat object:

1. `nFeature_RNA` - The number of genes detected per cell.

Too low - likely an empty or poor-quality cell

Too high - often doublets

2. `nCount_RNA` - The total number of reads per cell (library size).

Very low - failed cell

Very high - potential doublet or unusually deep sequencing

3. `percent.mt` - The proportion of reads mapping to mitochondrial genes.

High mitochondrial percentage indicates stressed or dying cells

To compute `percent.mt`, we identify mitochondrial genes (usually starting with "MT-") and calculate the fraction of counts they contribute.

After visualising these metrics, we decide on thresholds and remove low-quality cells.

Filtering is one of the most important steps: it ensures that downstream normalisation, clustering, and biological interpretation are meaningful.

Tasks:
* Visualise `nFeature_RNA` and `nCount_RNA`, please see https://satijalab.org/seurat/articles/pbmc3k_tutorial.html.

```
# Your code here: 

```

* Do you see outliers with very low or very high values?

* What could those outliers represent?

Identify mitochondrial genes, calculate percent.mt and plot it, please see https://satijalab.org/seurat/articles/pbmc3k_tutorial.html:

```
# Your code here: 


```

* Are there cells with unusually high mitochondrial content?

* Should they be filtered out?

```
# Replace 1 with the custom values from your data and plot the data after filtration:
seurat_obj <- subset(seurat_obj, subset = nFeature_RNA > 1 &
                                nFeature_RNA < 1 &
                                percent.mt < 1)

```

* Try different values above and print the violin plots to see how they change:

```
# Your code here:

```

# 4. Normalise data

After filtering out low-quality cells, the next essential step is normalisation.
In single-cell RNA-seq, each cell can have a very different sequencing depth:

* some cells have many counts (deeply sequenced)

* others have few counts

If we compare raw counts directly, we may falsely conclude that one cell expresses more genes simply because it was sequenced more deeply.

Normalisation fixes this.

In other words:
Because we do not know the true total RNA in each cell, normalisation adjusts for the number of reads we observed instead.
By scaling cells to the same “size” (same sequencing depth), we ensure that expression differences between cells reflect biology, not technical artefacts.

Seurat provides several approaches for normalising single-cell RNA-seq data. They differ in complexity and in the types of data they work best for. Seurat uses LogNormalize by default because it is simple, fast, and suitable for most introductory analyses. It preserves general gene–gene relationships while correcting for differences in sequencing depth.

Task:
* Use `NormalizeData()` to normalise your filtered Seurat object: use `names(seurat_obj[["RNA"]]@layers)` (for Seurat v5) to inspect the RNA assay layers before normalisation. After running `NormalizeData()`, inspect the layers again. What has changed?
Hint: https://satijalab.org/seurat/reference/normalizedata 

```
# Your code here: 

```

Questions and tasks:
* Why do we normalise after filtering, not before?
* Now you can also plot gene expression (hint: `VlnPlot()`).

```
# Your code here:

```


