---
layout: default
title: Run Vulture pipeline
parent: Nextflow
nav_order: 2
---

# Nextflow and scRNA-Seq processing
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---
## Setup the AWS CLI

Install the AWS CLI and prepare the access key and secret access key ahead by following instructions here https://docs.aws.amazon.com/powershell/latest/userguide/pstools-appendix-sign-up.html.

```shell
pip install awscli
aws configure
# here will prompt you to enter access key and secret access key
```

## Clone Vulture source code

Clone the source code of Vulture into your local computer

```shell
git clone https://github.com/hoyu310/scvh.gi
# change into directory below
cd scvh/nextflow
# checkout the branch below for most updated code
git checkout cloud-new-junyi
```

## Create S3 Bucket to store results

```shell
# specify bucket names and save them into bash environment variables
export BUCKET_NAME_TEMP=vulture-temp
export BUCKET_NAME_RESULTS=vulture-results
echo "BUCKET_NAME_TEMP=${BUCKET_NAME_TEMP}" |tee -a ~/.bashrc
echo "BUCKET_NAME_RESULTS=${BUCKET_NAME_RESULTS}" |tee -a ~/.bashrc
# create S3 buckets with specified bucket names 
aws --region ${AWS_REGION} s3 mb s3://${BUCKET_NAME_TEMP}
aws --region ${AWS_REGION} s3 mb s3://${BUCKET_NAME_RESULTS}
```

## Run Vulture pipeline - 1. Build Genome reference

Now we are about to run the first step of the Vulture pipeline i.e. mkref (genome reference making), execute the command below and wait it to be finished

```shell
nextflow run scvh_mkref.nf -profile mkref -bucket-dir s3://${BUCKET_NAME_TEMP} --outdir=s3://${BUCKET_NAME_RESULTS}/batchA -with-report mkref_$(date +%s).html -bg &>> mkref_$(date +%s).log;
```

After the above job is done, you need to edit line in "scvh/nextflow/nextflow.config" file -> params.ref = 's3://scvhwf/humangenome/' to the actual S3 path where your genome file is i.e. in "s3://${BUCKET_NAME_RESULTS}/batchA"

```shell
...
mkref {
aws.region = 'us-east-2'
process.container = 'public.ecr.aws/b6a4h2a6/scvh_mkref:latest'
process.executor = 'awsbatch'
process.queue = 'jy-scvh-queue-r5a4x-1'
# this line needs to be changed
params.ref = 's3://scvhwf/humangenome/'
}
...
```

## Run Vulture pipeline - 2. Start main analysis

Before we start our analysis, we need to edit "scvh/nextflow/params.yaml" file to include the reads of your interest. Here is a snippet of how the "params.yaml" file looks like

```shell
...
soloStrand: "Forward"
alignment: "STAR"
technology: "10XV2"
virus_database: "viruSITE"
soloMultiMappers: "EM"
soloFeatures: "GeneFull"
inputformat: "bam"
soloInputSAMattrBarcodeSeq: "CB UB"
barcodes_whitelist: "None"
reads: 
- "SRR6885502"
- "SRR6885503"
- "SRR6885504"
- "SRR6885505"
- "SRR6885506"
- "SRR6885507"
- "SRR6885508"
...
```

Execute the command below to start the main analysis of Vulture.

```shell
nextflow run scvh_full.nf -profile batchfull -params-file params.yaml -bucket-dir s3://${BUCKET_NAME_TEMP} --outdir=s3://${BUCKET_NAME_RESULTS}/batchD -with-report report_bam_$(date +%s).html -bg &>> submitnf_bam_$(date +%s).log
```

<div class="code-example" markdown="1">
[Previous Step](https://juychen.github.io/docs/3_Nextflow/NextflowInstall.html){: .btn }
[Next Step](https://juychen.github.io/docs/4_Batch/Batch.html){: .btn .btn-purple }
</div>