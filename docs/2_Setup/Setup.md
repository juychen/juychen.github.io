---
layout: default
title: Setup
nav_order: 2
has_children: true
---

To get started with Nextflow we are going to run a little example workflow locally.

# Setup
{: .no_toc }
This chapter introduces the AWS services you will use as part of the workshop. It is recommended that you familiarize yourself with these services before proceeding. However, step-by-step guidance throughout the workshop will include necessary context for these AWS services.

## Clone the tutorial

We will use the code from our repository. It contains the nextflow scripts and the required

```shell
git clone https://github.com/juychen/aws-workshop.git
cd aws-workshop
```

## Nextflow Config

As we are using Docker to execute the pipelines we will predefine the execution engine.

```shell
cat << \EOF > $HOME/.nextflow/config
docker.enabled = true
EOF
```

<div class="code-example" markdown="1">
[Previous Step](http://example.com/){: .btn }
[Next Step](https://juychen.github.io/docs/Setup/Cloud9.html){: .btn .btn-purple }
</div>