---
layout: default
title: Introduction
has_children: true
nav_order: 1
---

# Introduction
{: .no_toc }
This chapter introduces the AWS services you will use as part of the workshop. It is recommended that you familiarize yourself with these services before proceeding. However, step-by-step guidance throughout the workshop will include the necessary context for these AWS services.

## Container-based scRNA-Seq processing
This section introduces the run of docker image code using Nextflow in the local environment in EC2.Our docker is a simple image which installed [kallisto | bustools (KB)](https://www.kallistobus.tools) [1]. KB is a fast tool for pre-processing single-cell RNA-seq data. The reason we apply KB in the tutorial is that KB is relatively memory and computationally efficient compared with mainstream pre-processing tools. We have tested that it can run locally using a single "t3a.2xlarge" EC2 instance. Then we apply [Scanpy](https://scanpy.readthedocs.io/en/stable/) [2] to perform some basic analysis on the generated count matrix.

## Dataset

Our testing data are composed of two parts: the reference genome and the scRNA-Seq sample.

Our reference genome contains the human genome from hg38 and the virus genome from viruSITE. We used the command "kb ref" to build a kallisto index (transcriptome.idx) and transcript-to-gene mapping (transcript_to_gene.txt). They will be applied in the KB pseudoalignment of scRNA-Seq samples.

Our scRNA-Seq data (SRA Run selector accession number: [SRR11537951](https://www-ncbi-nlm-nih-gov.ezproxy.u-pec.fr/Traces/study/?acc=SRR11537951)) is a public available scRNA-Seq bronchoalveolar lavage fluid (BALF) from a sever COVID-19 patient provided by [Liao et al.](https://doi.org/10.1038/s41591-020-0901-9) [3]. The original size of the reads is 46.23 GB + 81.39 GB (R1 and R2). To reduce the running time of the tutorial, we subset reads from the original sample in two manners: 
- A smaller toy sample. Reards are randomly sampled from the original fastq file. The file size is 153.5 MB + 463.2 MB.
- A larger testing sample. Reads aligned to the human chromosome chr10 and the covid19 genome (NC_045512) are selected. The file size are 2.5 GB + 7.6 GB.


## References
1. Melsted, Páll, et al. "Modular, efficient and constant-memory single-cell RNA-seq preprocessing." Nature Biotechnology (2021): 1-6.
2. Wolf, F. Alexander, Philipp Angerer, and Fabian J. Theis. "SCANPY: large-scale single-cell gene expression data analysis." Genome Biology 19.1 (2018): 1-5.
3. Liao, Mingfeng, et al. "Single-cell landscape of bronchoalveolar immune cells in patients with COVID-19." Nature medicine 26.6 (2020): 842-844.

<div class="code-example" markdown="1">
[Previous Step](https://juychen.github.io){: .btn }
[Next Step](https://juychen.github.io/docs/1_Introduction/Account.html){: .btn .btn-purple }
</div>