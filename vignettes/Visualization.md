# Data Visualization
Yu-Jui Ho and Toby Aicher, M Hammell Lab  
`r Sys.Date()`  

Following identification of NMF clusters and sample assignemnts, SAKE provides several options for interactive data viusualization. Users can explore their NMF clusters through t-SNE and PCA projection plots. Users can also create standard gene expression heatmaps to, for example, evaluate gene expression patterns across samples in NMF clusters.

## t-SNE

[t-SNE](https://lvdmaaten.github.io/tsne/) is a non-linear form of dimensional reduction that gives each sample a location on a two or three dimensional grid. Early successful results of t-SNE maps in separating single cells of distinct origin have made t-SNE maps a popular choice for display of single-cell RNA-se data. The user can filter the genes used during t-SNE using four different ranking metrics: mean expression, median expression, MAD, and variance. Like for NMF, we recommend using using Top **1500 - 3000** MAD genes for `bulk RNA-Seq` data; Top **5000 - 8000** MAD genes for `single-cell RNA-Seq` data.  

Under more options, the user can further modify t-SNE:

- **Sample color:** the user can color the sample points either by filename, NMF group assignment, or the level of expression of a specified gene.

- **Perplexity:** perplexity is the number of neighbors used when computing t-SNE for each datapoint. A smaller perplexity will result in tighter clusters, while a higher perplexity will result in more diffuse clusters. 

- **Iterations:** t-SNE will run for a selected number of iterations and choose the optimal dimensional reduction. Generally, we found that the number of iterations did not greatly impact the t-SNE display.

As mentioned in the earlier section on NMF, concordance of NMF groups and t-SNE clusters indicate the robustness of both methods for identifying expresison clusters in RNA-seq datasets. It's important to use NMF clustering in addition to t-SNE visualization maps because NMF can help quantitatively assign data points to clusters that occupy distinct but closely connected t-SNE groupings.

SAKE provide t-SNE plots both in 2-D and 3-D for users to better understand the clustering results. 

<img src="Figures/Ting/NMF_Ting_K5_t-SNE.png" width="700px" height="380px" />


## PCA 

Principal component analysis (PCA) is a dimensional reduction technique that finds inter-related variables within data and reduces them into a smaller set of independent variables that explain most of the variance in the data. The principal components are ordered by the amount of variance in the data they explain (e.g. the first principal component explains the most variance in the data). The first two or three principal components can be used to visualize data by plotting data points using the principal components as axes. 

As with t-SNE and NMF, the user has the option to filter the number of genes used to calculate the principal components with four different ranking metrics: mean expression, median expression, MAD, and variance. We recommend using using Top **1500 - 3000** MAD genes for `bulk RNA-Seq` data; Top **5000 - 8000** MAD genes for `single-cell RNA-Seq` data.  

The user can choose which principal components to use as axes to visualize their data. The default is to use the first and second principal components for 2D PCA, and the first, second, and third axes for 3D PCA. 

The user can also designante the size of each sample dot, whether to display its label, the size of the label, and the alpha value (the transparency of each of the dots).

<img src="Figures/Ting/NMF_Ting_K5_PCA.png" width="700px" height="380px" />

## Heatmap

[Heatmaps](https://cran.r-project.org/web/packages/heatmap3/index.html) help with visualizing patterns in gene expression across multiple samples. Each column is a different sample and each row is a different gene. 

There are five options for selecting sets of genes to analyze:

- **Preloaded gene list:** the user can select from preloaded gene lists with gene signatures of various cell types and cell states derived from the published literature. 

- **Ranks from data\:** the user can filter a selected number of genes to analyze using four different metrics: median absolute deviation, median expression, mean expression, and variance. 

- **Manually select genes:** the user can select genes of interest that are present in the input data. This can be especially helpful for evaluating the patterns of known markers for a given cell type across NMF clustered samples. 

- **From NMF features:** the user can select genes that were found to be uniquely enriched in a particular NMF group. NMF enriched feature genes will usually be highly expressed in their respective NMF groups and poorly expressed in other samples. 


- **Upload gene list:** the user can upload their own gene list. 

An example gene list file should look like [this](../inst/extdata/genefile/genelist.EMT.txt):

Gene        | 
------------| -------------
AHNAK       |
BMP1        |
CALD1       |
CAMK2N1     |
CDH2        |
COL1A2      |
COL3A1      |
COL5A2      |
FN1         |

                     
The first row should be a character string **Gene**. The following rows should be the names/IDs of your gene of interest. 
     

An example heatmap using genes from NMF selected features is shown below. The color bar on the top of the heatmap indicates which NMF group each sample is assigned to.  

<img src="Figures/Ting/NMF_Ting_K5_heatmap.png" width="800px" height="550px" />

     
                
### More options

Under more options, the user can change the parameters of the heatmap.

- **Column and row colors:** the user can change how the rows and columns are colored at the top and sides of the heatmap. Under column colors, the user can color the sample columns by their filename or by their NMF group.

- **Heatmap color scheme:** the user can pick from several different color schemes for the heatmap itself. 

- **Column clustering:** the user can choose to cluster/order their samples based on NMF grouping, filenames, the expression of a particular gene, or a standard hierarchical clustering method. 

- **Distance and Linkage:** if hierarchical clustering is chosen for ordering the heatmap samples (columns) and genes (rows), the user may choose from several availale sample distance metrics and cluster linkage options. 


## Summary Stats

Several options are available to display summary statistics within NMF assigned groups. This includes:  transcriptome variance distributions, histograms of the number of expressed genes in each group, and boxplots of mean intra-group correlation coefficients.  Each of these distributions were calculated for each individual NMF group in order to assess the level of within- and between-cluster heterogeneity. Groups with high levels of intragroup heterogeneity are more likely to have high levels of transcriptome variance and low mean correlation coefficient. This may be due to sub-clusters present within a given group.  Alternately, such clusters may represent outlier samples that could include low quality samples, which often have fewer expressed genes overall relative to other groups. 

Based on these criteria for the samples displayed below, NMF group2 and group3 contained samples with higher levels of heterogeneity. A majority of the cells in NMF group2 were identified as deriving from a single cell type in the original author's publication, whereas cells in NMF group3 were identified as deriving from two distinct cell types (Ting et al., 2014).   


<img src="Figures/Ting/NMF_Ting_K5_groupstats.png" width="800px" height="300px" />


***

Continue on the next section [Differential Expression and Enrichment Analysis](DE_Enrich.Rmd)


