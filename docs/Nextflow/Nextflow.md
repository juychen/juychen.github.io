---
layout: default
title: Nextflow local run
permalink: docs/Nextflow
nav_order: 3
has_children: true
---
# Nextflow and scRNA-Seq processing
This section introduce the run of docker image code using nextflow in the local envirionment in EC2.Our docker is a simple image which installed [kallisto | bustools (KB)](https://www.kallistobus.tools). KB is a fast tool for pre-processing single-cell RNA-seq data. The reason we apply KB in the tutorial is that KB is relatively memeory and computational efficient compared with mainstream pre-processing tools. We have tested that it can run locally using a single "t3a.2xlarge" EC2 instance.

Attached is the Nextflow script we apply in this tutoruaial. It is a simple script using KB to align scRNA-Seq reads to KB indexed reference transcriptome (including transcripts_to_genes.txt and transcriptome.idx) and generate a count matrix. The Nextflow script will scan the input folder "data/" to find a pair of read files. Names of the input reads files should have the format of "(SRR run id)_1.fastq.gz" for barcode reads, and "(SRR run id)_2.fastq.gz" for cDNA reads. You can refer to the [Nextflow documentation](https://www.nextflow.io/docs/latest/getstarted.html) for more information about nextflow syntax and usage.



```shell
#!/usr/bin/env nextflow
/*
 * pipeline input parameters
 */
params.baseDir = "."
params.reads = "$baseDir/data/*_{2,1}.fastq.gz" 
params.outdir = "$baseDir/outputs"
params.refdir = "$baseDir/ref"
params.codebase = "~"
log.info """\
        - N F   P I P E L I N E -
         ===================================
         references   : ${params.refdir}
         reads        : ${params.reads}
         outdir       : ${params.outdir}
         """
         .stripIndent()


Channel
    .fromFilePairs( params.reads )
    .ifEmpty { error "Cannot find any reads matching: ${params.reads}" }
    .view()
    .set { read_pairs_ch }

/*
 * 1. Mapping
 */
process Map {
    
    publishDir params.outdir, mode: 'copy', overwrite: false

    input:
    tuple val(SRR_id), file(reads) from read_pairs_ch
    path ref from params.refdir
    output:
    val SRR_id into id_ch
    file("${SRR_id}/") into results_ch

    shell
    """
    kb count -x=10XV2 -g="${ref}/transcripts_to_genes.txt"  -i="${ref}/transcriptome.idx" -o="${SRR_id}" --tmp="~/kbtemp" --h5ad \
    "${params.baseDir}/${SRR_id}_1.fastq.gz" \
    "${params.baseDir}/${SRR_id}_2.fastq.gz" \
    """
}
```

Pull Image and download files
To run the docker image locally, we need to pull the docker image from the Amazon ECR 

```shell
docker pull public.ecr.aws/b6a4h2a6/kb_workshop:latest
```

Afterwards, we downlaod the read files and the reference from out S3 bucket using the following command:
```shell
aws s3 cp s3://awsscwsbucket/ref/transcripts_to_genes.txt ~/environment/aws-workshop/ref/transcripts_to_genes.txt & \
aws s3 cp s3://awsscwsbucket/ref/transcriptome.idx ~/environment/aws-workshop/ref/transcriptome.idx & \
aws s3 cp s3://awsscwsbucket/seqs/SRR11181959_2.fastq.gz ~/environment/aws-workshop/data/SRR11537951_1.fastq.gz &\
aws s3 cp s3://awsscwsbucket/seqs/SRR11181959_1.fastq.gz ~/environment/aws-workshop/data/SRR11537951_2.fastq.gz;
```

``` shell
nextflow  run script0.nf  
``` 

``` shell
N E X T F L O W  ~  version 21.10.0
Launching `script0.nf` [drunk_fermat] - revision: 6569bffc1c
- N F   P I P E L I N E -
 ===================================
 references   : /home/ec2-user/environment/aws-workshop/ref
 reads        : /home/ec2-user/environment/aws-workshop/data/*_{2,1}.fastq.gz
 outdir       : /home/ec2-user/environment/aws-workshop/outputs

[SRR11537951, [/home/ec2-user/environment/aws-workshop/data/SRR11537951_1.fastq.gz, /home/ec2-user/environment/aws-workshop/data/SRR11537951_2.fastq.gz]]
executor >  local (1)
[84/60ccf3] process > Map (1) [100%] 1 of 1 ✔
Completed at: 21-Nov-2021 08:58:11
Duration    : 4m 29s
CPU hours   : 0.1
Succeeded   : 1
```

<div class="code-example" markdown="1">
[Previous Step](https://juychen.github.io/docs/Setup/Cloud9IAM.html){: .btn }
[Next Step](http://example.com/){: .btn .btn-purple }
</div>