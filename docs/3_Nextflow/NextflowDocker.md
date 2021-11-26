---
layout: default
title: Nextflow local run preparation
parent: Nextflow
nav_order: 1
---

# Nextflow local run preparation
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Overview of the process

Attached is the Nextflow script we apply in this tutorial. It is a simple script using KB to align scRNA-Seq reads to KB indexed reference transcriptome (including transcripts_to_genes.txt and transcriptome.idx) and generate a count matrix. The Nextflow script will scan the input folder "data/" to find a pair of read files. Names of the input reads files should have the format of "(SRR run id)_1.fastq.gz" for barcode reads, and "(SRR run id)_2.fastq.gz" for cDNA reads. Then we apply scanpy to perform some basic analysis on the generated count matrix.

## Introduction to containers

A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another. A Docker container image is a lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries and settings.

The container (/aws-workshop/Dockerfile) in the presnet workshop look like this:

```docker
FROM python:3.7.5-slim
ARG KB_VERSION=0.24.4
COPY . /root
RUN apt-get update && \
	apt-get install --no-install-recommends -y curl dpkg-dev gnupg lsb-release procps
ENV PATH="/usr/local/bin:${PATH}"
RUN pip install --upgrade pip && pip install kb-python awscli scanpy[leiden]```
```

This is a simple container that with python packages "kb-python" "awscli" and "scanpy". A container is similar to a small virual machine. When you build a docker image, those commands in the dockerfile are command in the console helping you to setup softwares and environments. We have build a runnable docker image stored in the AWS ECR repositoy. You can download it by the following command: 


```shell
docker pull public.ecr.aws/b6a4h2a6/kb_workshop:latest
```

The consle will output some information after you correctlly pull an image like this:

```shell
latest: Pulling from b6a4h2a6/kb_workshop
000eee12ec04: Already exists 
ddc2d83f8229: Already exists 
735b0bee82a3: Already exists 
8c69dcedfc84: Already exists 
495e1cccc7f9: Already exists 
d7c2e86e32fa: Pull complete 
5aca37fc9282: Pull complete 
4a8de34bdc8d: Pull complete 
Digest: sha256:31e1c06c105c0471e99eb87631731fdcd08758ff7aa0c3f11709104dd31d9387
Status: Downloaded newer image for public.ecr.aws/b6a4h2a6/kb_workshop:latest
public.ecr.aws/b6a4h2a6/kb_workshop:latest
```

Afterwards, we downlaod the read files and the reference from out S3 bucket using the following command:

```shell
aws s3 cp s3://awsscwsbucket/ref/transcripts_to_genes.txt ~/environment/aws-workshop/ref/transcripts_to_genes.txt & \
aws s3 cp s3://awsscwsbucket/ref/transcriptome.idx ~/environment/aws-workshop/ref/transcriptome.idx & \
aws s3 cp s3://awsscwsbucket/seqs/SRR11537951_toy/SRR11537951_1.fastq.gz ~/environment/aws-workshop/data/SRR11537951_1.fastq.gz &\
aws s3 cp s3://awsscwsbucket/seqs/SRR11537951_toy/SRR11537951_2.fastq.gz ~/environment/aws-workshop/data/SRR11537951_2.fastq.gz;
```

``` shell
nextflow  run script0.nf  
``` 

``` shell
N E X T F L O W  ~  version 21.10.3
Launching `script0.nf` [serene_euclid] - revision: 03d5e629a6
- N F   P I P E L I N E -
 ===================================
 references   : /home/ec2-user/environment/aws-workshop/ref
 reads        : /home/ec2-user/environment/aws-workshop/data/*_{2,1}.fastq.gz
 outdir       : /home/ec2-user/environment/aws-workshop/outputs

[SRR11537951, [/home/ec2-user/environment/aws-workshop/data/SRR11537951_1.fastq.gz, /home/ec2-user/environment/aws-workshop/data/SRR11537951_2.fastq.gz]]
executor >  local (2)
[01/79910a] process > Map (1)      [100%] 1 of 1 ✔
[96/da8bfe] process > Analysis (1) [100%] 1 of 1 ✔
Completed at: 26-Nov-2021 09:01:21
Duration    : 4m 49s
CPU hours   : 0.1
Succeeded   : 2
```

## Redirect results to the S3 bucket

Create our S3 bucket to store the result. 
``` shell
export BUCKET_NAME_RESULTS=nextflow-spot-batch-result-${RANDOM}-$(date +%s)
aws --region ${AWS_REGION} s3 mb s3://${BUCKET_NAME_RESULTS}
aws s3api put-bucket-tagging --bucket ${BUCKET_NAME_RESULTS} --tagging="TagSet=[{Key=nextflow-workshop,Value=true}]"
echo ${BUCKET_NAME_RESULTS}
```
In this step, you will create a bucket on the AWS S3. It will have a fold name like "nextflow-spot-batch-result-14962-1637501981". Remember to **Remember the AWS bucket location**. This location will be the path that store your experiment result. Also, this buck will be used in the following step. The above command will show the result of the result bucket.

Here, we can run the nextflow script again.

``` shell
nextflow run script0.nf --outdir=s3://${BUCKET_NAME_RESULTS}/outputs
```

Then, we can list the result in the S3 bucket

``` shell
aws s3 ls ${BUCKET_NAME_RESULTS}/outputs/SRR11537951/
```

The result should be somthing similar to this.
``` shell
workshop_user0:~/environment/aws-workshop (main) $ aws s3 ls ${BUCKET_NAME_RESULTS}/outputs/SRR11537951/
                           PRE counts_unfiltered/
2021-11-21 13:44:59          0 
2021-11-21 13:45:09   12533760 10x_version2_whitelist.txt
2021-11-21 13:45:09        574 inspect.json
2021-11-21 13:45:17       2078 kb_info.json
2021-11-21 13:45:06   83672160 matrix.ec
2021-11-21 13:45:02  126870353 output.bus
2021-11-21 13:45:12   77054193 output.unfiltered.bus
2021-11-21 13:45:08        454 run_info.json
2021-11-21 13:45:08   18376572 transcripts.txt
```
<div class="code-example" markdown="1">
[Previous Step](https://juychen.github.io/docs/Setup/Cloud9IAM.html){: .btn }
[Next Step](http://example.com/){: .btn .btn-purple }
</div>