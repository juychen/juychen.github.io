---
layout: default
title: Batch
has_children: true
nav_order: 4
---

# Overview
{: .no_toc }

AWS Batch enables developers, scientists, and engineers to easily and efficiently run hundreds of thousands of batch computing jobs on AWS. AWS Batch dynamically provisions the optimal quantity and type of computational resources (e.g., CPU or memory optimized instances) based on the volume and specific resource requirements of the batch jobs submitted. With AWS Batch, there is no need to install and manage batch computing software or server clusters that you use to run your jobs, allowing you to focus on analyzing results and solving problems. AWS Batch plans, schedules, and executes your batch computing workloads across the full range of AWS compute services and features, such as AWS Fargate, Amazon EC2, and Spot Instances.

There is no additional charge for AWS Batch. You only pay for the AWS resources (e.g. EC2 instances) you create to store and run your batch jobs.

In our tutorial, the pipeline of Nextflow and Batch deal with jobs by the following workflow automatically：

1. We configure and run Nextflow script to trigger a submission to the job queue.
2. The AWS Batch scheduler runs periodically to check for jobs in the job queue
3. Once jobs are placed, the scheduler evaluates the Compute Environments and adjusts
the compute capacity to allow the jobs to run.
4. The job (packaged by the container) will run in the computational environment created.
5. The job output will be transferred and stored to the S3 bucket.

<div class="code-example" markdown="1">
[Previous Step](https://juychen.github.io/docs/3_Nextflow/Nextflow.html){: .btn }
[Next Step](https://juychen.github.io/docs/4_Batch/BatchEnvironment.html){: .btn .btn-purple }
</div>