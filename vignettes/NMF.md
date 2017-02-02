# Non-negative Matrix Factorization
Yu-Jui Ho and Toby Aicher, M Hammell Lab  
`r Sys.Date()`  

The central part of the SAKE package uses non-negative matrix factorization (NMF) to decompose a gene expression matrix V into two non-negative matrices, W and H, via a multiplicative updates algorithm. NMF was originally developed to be used in image analysis and language processing^[Lee and Seung, Learning the parts of objects by non-negative matrix factorization, Nature, 1999]. More recently, it has been sucessfully applied to the field of computational biology as an unsupervised clustering method that helps classify samples/patients into functional groups in an unbiased manner^[Genomic Classification of Cutaneous Melanoma., Cell, 2015].

For running NMF:

- Requires an input matrix to of non-negative expression values
- Requires a two-step procedure where users first identify the most likely number of clusters present in the dataset and then identify which samples belong to each expression cluster. 


## Estimate Number of Clusters

The first step of running NMF is to decide the number of clusters (K) present in the data. In order to do so, we  run NMF simultaneously with several different values of K, then use the supplied graphical representations of distance metrics to pick the value of K that best fits the underlying data structure. SAKE relies on the published [NMF](http://renozao.github.io/NMF/devel/index.html) R package of Gaujoux & Seoighe (2010). We have obtained robust results by choosing the value, K, at which the *cophenetic* coefficient begins to drop, as suggested in Brunet et al (2004). The cophenetic coefficient measures the similarity between samples within a single cluster relative to similarities between that sample and other samples not in the same cluster, with higher cophenetic coefficients corresponding to higher within-cluster similarity. We also suggest investigating the distribution of the *silhouette index* and picking the K with the highest value. 

An example result is shown below for the consensus plots and distributions of cophenetic coefficients and silhouette index for different values of K using a published single-cell RNA-seq dataset^[Ting *et al*, Single-Cell RNA Sequencing Identifies Extracellular Matrix Gene Expression by Pancreatic Circulating Tumor Cells, Cell Reports, 2014] is displayed below. In this case, the estimated number of clusters present in the data is **5**, as indicated by the consensus plots, cophenetic coefficients, and silhouette index. 

<img src="./Figures/Ting/NMF_Ting_EstimK_consensus.png" width="800px" height="600px" />
<img src="./Figures/Ting/NMF_Ting_EstimK_stats.png" width="800px" height="600px" />

*Note: It takes around 20 minutes to run 20 iterations for each value of K on a MacBook Pro (Retina, 15-inch, Mid 2015), 2.5 GHz Intel Core i7, with 16 GB 1600 MHz DDR3*

## Run NMF 

After determining the value of **K**, we suggest running NMF for several randomized iterations using that value of K in order to estimate the robustness of cluster marker genes and cluster membership of each sample.

* **Number of runs** - The [NMF](http://renozao.github.io/NMF/devel/index.html) package suggests using `20-30` runs for estimating the number of K and using `50-100` runs for a final NMF set of iterations. 
* **Initial seed number** - You can specify a seed number for a deterministic NMF run result. Otherwise, random seeds will be generated for each NMF run, which may give slightly different results for small run numbers. The default is to set the seed to `123211`. 

*Note: For the case displayed below, we  have used 50 iterations for a demostration NMF run. It is suggested to run `50-100` iterations for more robust results. It takes around 6 minutes to finish 50 iterations on a MacBook Pro (Retina, 15-inch, Mid 2015), 2.5 GHz Intel Core i7, with 16 GB 1600 MHz DDR3* 

## Identify Groups

Following the final NMF run, NMF group assignment for each sample is displayed on the left; while a [t-SNE](https://lvdmaaten.github.io/tsne/) plot coloring each sample by NMF-assigned group is displayed on the right. The size of the dot used for each sample can be adjusted proportionally to the probability of that sample being assigned to the most appropriate NMF group. The probability of correct assignment for each sample is estimated by calculating the loading weight of that sample in the assigned group and dividing by the sum of the total loading weights for that sample in all other groups. A higher probability represents higher confidence that a sample has been robustly assigned to the correct group.

Usually, samples from the same NMF group form tight clusters on the t-SNE plot. This indicates high agreement between two independent and robust methods of calculating sample similarity. In some cases, NMF will separate samples into different groups, while t-SNE indicates that these samples occupy similar but distinct areas in the t-SNE projection plot, such as the red, yellow, and blue color samples displayed below. From the t-SNE plot alone, we would not assume that these samples represented distinct clusters.  In this example, these samples do indeed derive from distinct cell types highlighting the strength of NMF in classifying related samples into distinct clusters. 

####

<img src="./Figures/Ting/NMF_Ting_K5_Group.png" width="800px" height="500px" />  

## Enriched Features

The `feature` tab includes the enriched features (gene markers) in each NMF-assigned group. For each gene, a `featureScore` will be calculated indicating the relative specificity of that gene in separating clusters from each other.  Then based on `Kim & Park's` feature selection method (2007), only the genes with featureScores that are greater than 3 median absolute deviations (MAD) away from all other featureScores will be selected as markers for each group. 

Genes are ordered by their featureScore ranks in each group. Users can click on the name of the gene of interest, which will link to the `GeneCards`^[GeneCards: http://www.genecards.org] page with more detailed information. Users can also click on the gene row (as highlighted in the figure below), which will display a boxplot of the gene expression values across samples in each NMF group, at right. In this example, `Gp9` is an enriched feature identified in `NMF group1`, therefore its expression value is expected to be generally highest in `NMF group1`. 

<img src="./Figures/Ting/NMF_Ting_K5_Feature.png" width="800px" height="500px" />  

## More Information

Users are encouraged to read more about the methods and implementation of [NMF](http://renozao.github.io/NMF/devel/index.html). 

***

Continue on the next section [Visualization](Visualization.Rmd)
