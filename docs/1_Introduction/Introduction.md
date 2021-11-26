---
layout: default
title: Introduction
nav_order: 1
permalink: docs/Introduction
---

# Introduction
{: .no_toc }
This chapter introduces the AWS services you will use as part of the workshop. It is recommended that you familiarize yourself with these services before proceeding. However, step-by-step guidance throughout the workshop will include necessary context for these AWS services.

## Container based scRNA-Seq processing
This section introduce the run of docker image code using nextflow in the local envirionment in EC2.Our docker is a simple image which installed [kallisto | bustools (KB)](https://www.kallistobus.tools) [1]. KB is a fast tool for pre-processing single-cell RNA-seq data. The reason we apply KB in the tutorial is that KB is relatively memeory and computational efficient compared with mainstream pre-processing tools. We have tested that it can run locally using a single "t3a.2xlarge" EC2 instance. Then we apply [Scanpy](https://scanpy.readthedocs.io/en/stable/) [2] to perform some basic analysis on the generated count matrix.


## References
1. Melsted, Páll, et al. "Modular, efficient and constant-memory single-cell RNA-seq preprocessing." Nature biotechnology (2021): 1-6.

2. Wolf, F. Alexander, Philipp Angerer, and Fabian J. Theis. "SCANPY: large-scale single-cell gene expression data analysis." Genome biology 19.1 (2018): 1-5.

<div class="code-example" markdown="1">
[Previous Step](https://juychen.github.io){: .btn }
[Next Step](https://juychen.github.io/docs/2_Setup/Setup.html){: .btn .btn-purple }
</div>