# Differential Expression and Enrichment Analysis
Yu-Jui Ho and Toby Aicher, M Hammell Lab  
`r Sys.Date()`  

Differential expression (DE) analysis between NMF clusters allows for the identification of additional genes differentially expressed in each NMF cluster with statistical analyses calculated via the [DESeq](http://bioconductor.org/packages/release/bioc/html/DESeq.html) algorithm (Love et al., 2014). Expression distributions for DE genes across NMF groups are displayed together with RefSeq annotation. 

## Differential Expression 

<img src="Figures/Ting/NMF_Ting_K5_DESeq2_Group1.png" width="800px" height="450px" />

<img src="Figures/Ting/NMF_Ting_K5_DESeq2_Group3.png" width="800px" height="450px" />



## Enrichment Analysis 

[GO Term enrichments](http://bioconductor.org/packages/devel/bioc/html/gage.html) allow for the identification of functional categories enriched in each NMF cluster, which can serve as guidance for further investigation and follow up studies. 

*Note: Make sure to select the correct species. SAKE currently supports `Human` and `Mouse`.*

<img src="Figures/Ting/NMF_Ting_K5_Enrich_Sel_Mouse.png" width="800px" height="450px" />

Summary table and plots for the enrichment analysis. 

<img src="Figures/Ting/NMF_Ting_K5_Enrich_Res.png" width="800px" height="400px" />

User can also click on one of the enriched KEGG pathways they are interested in. [Pathview](https://bioconductor.org/packages/release/bioc/html/pathview.html) will maps and renders pathway grpahs. In this case, we select `TGF-beta signaling pathway` from `NMF Group3`. Red color indicates genes that are upregulated in selected NMF cluster as compared to all the other clusters. 

<img src="Figures/Ting/NMF_Ting_K5_Pathview.png" width="800px" height="500px" />
