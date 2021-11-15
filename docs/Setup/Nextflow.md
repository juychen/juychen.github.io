---
layout: default
title: Nextflow
parent: Setup
---
# Nextflow
This section introduce the installation of nextflow and its dependencies.

## Amazon Corretto

To install Corretto , we are adding the repository first.

```shell
sudo rpm --import https://yum.corretto.aws/corretto.key
sudo curl -L -o /etc/yum.repos.d/corretto.repo https://yum.corretto.aws/corretto.repo
```

Afterwards install java-11 and check the installation.

```shell
sudo yum install -y java-11-amazon-corretto-devel
java --version
```

## Graphviz

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