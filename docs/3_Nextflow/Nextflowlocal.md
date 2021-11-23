---
layout: default
title: Nextflow local run
parent: Nextflow
nav_order: 1
---
# Nextflow and scRNA-Seq processing

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

## Redirect results to the S3 bucket

Create our S3 bucket to store the result. 
``` shell
export BUCKET_NAME_RESULTS=nextflow-spot-batch-result-${RANDOM}-$(date +%s)
aws --region ${AWS_REGION} s3 mb s3://${BUCKET_NAME_RESULTS}
aws s3api put-bucket-tagging --bucket ${BUCKET_NAME_RESULTS} --tagging="TagSet=[{Key=nextflow-workshop,Value=true}]"
aws s3 ls
```
Remember the AWS bucket location and this location will be the path that store your experiment result. The above command will show the result of the result bucket.

Here, we can run the nextflow script again.

``` shell
nextflow run script0.nf --outdir=s3://${BUCKET_NAME_RESULTS}/outputs
```

List outputs

``` shell
aws s3 ls ${BUCKET_NAME_RESULTS}/outputs/SRR11537951/
```

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

Check result
<div class="code-example" markdown="1">
[Previous Step](https://juychen.github.io/docs/Setup/Cloud9IAM.html){: .btn }
[Next Step](http://example.com/){: .btn .btn-purple }
</div>