---
layout: default
title: Batch Job Queue
parent: Batch
nav_order: 2
---

# Create Batch Job Queue
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Setup Batch Compute Environment

We can return back to the home page of Batch at: [https://us-east-2.console.aws.amazon.com/batch/home](https://us-east-2.console.aws.amazon.com/batch/home).

- Select "Job queues" on the left panel
- Select "Create" to create a new job queue

![Image](../../src/img/Batch/Batch-queue1.jpg)

- Name your job queue to "job-queue-userxxx" according to your user name.

![Image](../../src/img/Batch/Batch-queue2.jpg)

- Select your compute environment to "spot-ce-userxxx" which you created in the previous step

![Image](../../src/img/Batch/Batch-queue3.jpg)

![Image](../../src/img/Batch/Batch-env4.jpg)

- Select "Create" to create a new job queue


<div class="code-example" markdown="1">
[Previous Step](https://juychen.github.io/docs/4_Batch/BatchEnvironment.html){: .btn }
[Next Step](https://juychen.github.io/docs/4_Batch/BatchNextflow.html){: .btn .btn-purple }
</div>