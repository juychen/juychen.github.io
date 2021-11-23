---
layout: default
title: Batch - Environments
parent: Batch
nav_order: 1
---

# Create AWS Batch Environmnet
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Setup Batch Envirionment

Before we start, we need to setup a launch template of EC2 instance for our computing environments for AWS batch. Because we have mentioned in the previous section that the storage of EC2 instance is not enough for the scRNA-Seq preprocessing.

The launch template file in our project (launch-template-data.json) is shown as follows:
```json
{
    "LaunchTemplateName": "increase-volume",
    "LaunchTemplateData": {
        "BlockDeviceMappings": [
            {
                "DeviceName": "/dev/xvda",
                "Ebs": {
                    "VolumeSize": 100,
                    "VolumeType": "gp2"
                }
            }
        ]
    }
}
```
With this template name "increase-volume", we can encrease the default storage of EC2 instances to 100GB. We can run the following script: 

```shell
aws ec2 --region ${AWS_REGION} create-launch-template --cli-input-json file://launch-template-data.json
```



This command will creat a t
Create an AWS Batch Environment at: [https://us-east-2.console.aws.amazon.com/batch/home](https://us-east-2.console.aws.amazon.com/batch/home).

- Select "Create"
- Name it "aws-workshop", and take all other defaults

<div class="code-example" markdown="1">
[Previous Step](https://juychen.github.io/docs/Setup){: .btn }
[Next Step](https://juychen.github.io/docs/Setup/Cloud9IAM.html){: .btn .btn-purple }
</div>
