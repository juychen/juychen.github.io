---
layout: default
title: Setup Nextflow
parent: Setup
nav_order: 3
---
# Nextflow
This section introduce the installation of nextflow and its dependencies.

## Clone the tutorial

We will use the code from our repository. It contains the nextflow scripts and the required

```shell
git clone https://github.com/juychen/aws-workshop.git
cd aws-workshop
```

## Graphviz and jq

Nextflow is able to render graphs for which it needs graphviz to be installed. jq will help us deal with JSON files.

```shell
sudo yum install -y graphviz jq
```

## AWS Region

Even though we are depending on an IAM Role and not local permissions some tools depend on having the AWS_REGION defined as environment variable - let's add it to our login shell configuration.


```shell
export AWS_REGION=$(curl --silent http://169.254.169.254/latest/dynamic/instance-identity/document | jq -r .region)
echo "AWS_REGION=${AWS_REGION}" |tee -a ~/.bashrc
```

## Nextflow

Installing Nextflow using an online installer. The snippet creates the Nextflow launcher in the current directory. So we just move the command to /usr/local/bin to have it ready to be executed anywhere.


```shell
curl -s https://get.nextflow.io | bash
sudo mv nextflow /usr/local/bin/
```

<div class="code-example" markdown="1">
[Previous Step](https://juychen.github.io/docs/Setup/Cloud9IAM.html){: .btn }
[Next Step](http://example.com/){: .btn .btn-purple }
</div>