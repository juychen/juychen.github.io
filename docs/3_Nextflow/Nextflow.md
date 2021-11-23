---
layout: default
title: Nextflow
has_children: true
nav_order: 3
---
# Nextflow and scRNA-Seq processing
This section introduce the run of docker image code using nextflow in the local envirionment in EC2.Our docker is a simple image which installed [kallisto | bustools (KB)](https://www.kallistobus.tools). KB is a fast tool for pre-processing single-cell RNA-seq data. The reason we apply KB in the tutorial is that KB is relatively memeory and computational efficient compared with mainstream pre-processing tools. We have tested that it can run locally using a single "t3a.2xlarge" EC2 instance.

Check result
<div class="code-example" markdown="1">
[Previous Step](https://juychen.github.io/docs/Setup/Cloud9IAM.html){: .btn }
[Next Step](http://example.com/){: .btn .btn-purple }
</div>