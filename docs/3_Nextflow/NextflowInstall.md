---
layout: default
title: Install Nextflow
parent: Nextflow
nav_order: 1
---
# Nextflow
This section introduces the installation of Nextflow and its dependencies.

## Install Nextflow

Nextflow can be installed by following the instructions at [Nextflow Installation Guide](https://www.nextflow.io/docs/latest/getstarted.html). If you are using OS X/Linux, you can run the commands below to install it immediately.

```shell
wget -qO- https://get.nextflow.io | bash
# run from current directory
chmod +x nextflow
# run from anywhere
sudo mv nextflow /usr/local/bin/
```

## Install Nextflow dependencies for visualization

Nextflow is able to render graphs for which it needs graphviz to be installed. jq will help us deal with JSON files.

```shell
sudo yum install -y graphviz jq
```

## AWS Region

Even though we are depending on an IAM Role and not local permissions some tools depend on having the AWS_REGION defined as an environment variable - let's add it to our login shell configuration.


```shell
export AWS_REGION=us-east-2
echo "AWS_REGION=${AWS_REGION}" |tee -a ~/.bashrc
```

<div class="code-example" markdown="1">
[Previous Step](https://juychen.github.io/docs/3_Nextflow/Nextflow.html){: .btn }
[Next Step](https://juychen.github.io/docs/4_Batch/Batch.html){: .btn .btn-purple }
</div>
