---
layout: default
title: AWS Batch
nav_order: 4
has_children: true
---

A typical AWS Batch workload might be triggered by input data being uploaded to S3, this will kick in multiple stages.


# Overview
{: .no_toc }
1. The Upload event triggers a submission to a job queue (e.g. via lambda function)
2. The AWS Batch scheduler runs periodically to check for jobs in the job queue
3. Once jobs are placed, the scheduler evaluates the Compute Environments and adjusts
the compute capacity to allow the jobs to run.
5. The job output is stored external to the job (e.g. S3, EFS or FSx for Lustre)


<div class="code-example" markdown="1">
[Previous Step](http://example.com/){: .btn }
[Next Step](https://juychen.github.io/docs/Setup/Cloud9.html){: .btn .btn-purple }
</div>