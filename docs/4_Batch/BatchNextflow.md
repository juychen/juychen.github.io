---
layout: default
title: Nextflow - batch
parent: Batch
nav_order: 4
---
# Nextflow on Batch

## Update Configuration 
Now that we have created queues and compute environments, we can wire them into Nextflow.

Nextflow will evaluate a nextflow.config file next to the script we are executing (which would be the file in the current directory) and also fall back to $HOME/.nextflow/config for additional configuration. As we are going to use the latter one when using AWS Batch Squared we are changing both.

Firstly, the user-specific Nextflow configuration file.


```shell
cd ~/environment/nextflow-tutorial/
cat > $HOME/.nextflow/config  << EOF
profiles {
  standard {
    process.container = 'public.ecr.aws/b6a4h2a6/kb_workshop:latest'
    docker.enabled = true
  }

  batch {
    aws.region = '${AWS_REGION}'
    process.container = 'public.ecr.aws/b6a4h2a6/kb_workshop:latest'
    process.executor = 'awsbatch'
    process.queue = 'job-queue-userxxx'
  }
}
EOF
```

Remember to rename 'job-queue-userxxx' to the name of your own job queue.

Nextflow will evaluate a nextflow.config file next to the script we are executing (which would be the file in the current directory) and also fall back to $HOME/.nextflow/config for additional configuration. As we are going to use the latter one when using AWS Batch squared we are changing both. Thus, we are going to change the Nextflow configuration files.

## Create result S3 Bucket

Remember that have a S3 bucket that keeps the results in ${BUCKET_NAME_RESULTS}.

```shell
echo ${BUCKET_NAME_RESULTS}
nextflow-spot-batch-result-20033-1587976463
```

Here we would like to depoly another bucket to store the temporary files that AWS batch need to run in AWS Batch

```shell
export BUCKET_NAME_TEMP=nextflow-spot-batch-temp-${RANDOM}-$(date +%s)
aws --region ${AWS_REGION} s3 mb s3://${BUCKET_NAME_TEMP}
aws s3api put-bucket-tagging --bucket ${BUCKET_NAME_TEMP} --tagging="TagSet=[{Key=nextflow-workshop,Value=true}]"
echo ${BUCKET_NAME_TEMP}
```
"nextflow-spot-batch-result-14962-1637501981". **Remember the AWS bucket location** again.


## AWS Region

Even though we are depending on an IAM Role and not local permissions some tools depend on having the AWS_REGION defined as environment variable - let's add it to our login shell configuration.


```shell
export AWS_REGION=$(curl --silent http://169.254.169.254/latest/dynamic/instance-identity/document | jq -r .region)
echo "AWS_REGION=${AWS_REGION}" |tee -a ~/.bashrc
```

## Nextflow

Installing Nextflow using an online installer. The snippet creates the Nextflow launcher in the current directory. So we just move the command to /usr/local/bin to have it ready to be executed anywhere.


```shell
nextflow run script1.nf -profile batch -bucket-dir s3://${BUCKET_NAME_TEMP} --outdir=s3://${BUCKET_NAME_RESULTS}/batch
```
The output is going to look similar to this:

```shell
N E X T F L O W  ~  version 20.10 .0
Launching `script7.nf` [jovial_jones] - revision: ce58523d1d
R N A S E Q - N F   P I P E L I N E
===================================
transcriptome: /home/ec2-user/environment/nextflow-tutorial/data/ggal/transcriptome.fa
reads        : /home/ec2-user/environment/nextflow-tutorial/data/ggal/gut_{1,2}.fq
outdir       : s3://nextflow-spot-batch-result-23641-1587713021
WARN: Unable to create AWS Batch helper class | credentials cannot be null
executor >  awsbatch (4)
[2b/641a4f] process > index          [100%] 1 of 1 ✔
[f0/a87531] process > quantification [100%] 1 of 1 ✔
[08/014db2] process > fastqc         [100%] 1 of 1 ✔
[a1/ced1b8] process > multiqc        [100%] 1 of 1 ✔
Done! Open the following report in your browser --> s3://nextflow-spot-batch-result-23641-1587713021/batch/multiqc_report.html
Completed at: 24-Apr-2020 08:15:40
Duration    : 2m 42s
CPU hours   : (a few seconds)
Succeeded   : 4
```

## Monitoring Jobs

Head to the AWS Batch [dashboard](https://aws.amazon.com/batch/home#dashboard).


<div class="code-example" markdown="1">
[Previous Step](https://juychen.github.io/docs/Setup/Cloud9IAM.html){: .btn }
[Next Step](http://example.com/){: .btn .btn-purple }
</div>