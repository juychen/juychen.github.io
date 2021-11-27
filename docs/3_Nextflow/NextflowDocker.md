---
layout: default
title: Nextflow with container
parent: Nextflow
nav_order: 1
---

# Container prepration for Nextflow 
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Overview of the process

Attached is the Nextflow script we apply in this tutorial. It is a simple script using KB to align scRNA-Seq reads to KB indexed reference transcriptome (including transcripts_to_genes.txt and transcriptome.idx) and generate a count matrix. The Nextflow script will scan the input folder "data/" to find a pair of read files. Names of the input read files should have the format of "(SRR run id)_1.fastq.gz" for barcode reads, and "(SRR run id)_2.fastq.gz" for cDNA reads. 
## Introduction to containers

![Image](../../src/img/Nextflow/Nextflow-docker-1.png)

A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another. A Docker container image is a lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries, and settings.

The container (/aws-workshop/Dockerfile) in the present workshop look like this:

```docker
FROM python:3.7.5-slim
ARG KB_VERSION=0.24.4
COPY . /root
RUN apt-get update && \
	apt-get install --no-install-recommends -y curl dpkg-dev gnupg lsb-release procps
ENV PATH="/usr/local/bin:${PATH}"
RUN pip install --upgrade pip && pip install kb-python awscli scanpy[leiden]
```

This is a simple container with python packages "kb-python" "awscli" and "scanpy". A container is similar to a small virtual machine. When you build a docker image, those commands in the docker file are commands in the console helping you to set up software and environments. We have built Docker images stored in the AWS ECR repository. You can download it by the following command: 


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

Afterward, you can try to head into the container we pull to look around using the following command:

```shell
docker run -it public.ecr.aws/b6a4h2a6/kb_workshop:latest /bin/bash
```

For more information, you can head to the documentation of the [docker run](https://docs.docker.com/engine/reference/commandline/run/) command page.
This is a command that creates a writeable container layer over the specified image (public.ecr.aws/b6a4h2a6/kb_workshop:latest), and then starts it using the specified command (/bin/bash). After we run this command, we have been into 'another operating system.' We can type a KB help command to check whether we are in another system because we did not install kb in the EC2 instance.

```shell
kb --help
```

If the result is something like:

```shell
usage: kb [-h] [--list] <CMD> ...

kb_python 0.26.4

positional arguments:
  <CMD>
    info      Display package and citation information
    ref       Build a kallisto index and transcript-to-gene mapping
    count     Generate count matrices from a set of single-cell FASTQ files

optional arguments:
  -h, --help  Show this help message and exit
  --list      Display list of supported single-cell technologies
```

Remember that we have not installed the KB software in our EC2 instance. Therefore this KB information implies that you have correctly pulled and run the image. You can exit the container using the exit command:

```shell
root@e6d2ce35c6dc:/# exit
exit
workshop-user-00:~/environment/aws-workshop (main) $ 
```

Then you will come back to the operation system of the EC2 instance.
<div class="code-example" markdown="1">
[Previous Step](https://juychen.github.io/docs/3_Nextflow/Nextflow.html){: .btn }
[Next Step](https://juychen.github.io/docs/3_Nextflow/Nextflowlocal.html){: .btn .btn-purple }
</div>