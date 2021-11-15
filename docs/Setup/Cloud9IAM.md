---
layout: default
title: Cloud9 - IAM Settings
parent: Setup
nav_order: 2
---

# Cloud9 IAM settings
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Create an IAM role for your Workspace

Head over to the IAM console [Link](https://console.aws.amazon.com/iam/home) and find create role ([2]) under the Roles ([1]) section.

- Select Create environment
- Name it "aws-workshop", and take all other defaults

Your workspace should now look like this:

![Image](../../src/img/Setup/Cloud9-5.jpg)

## Increase the storage of the Cloud9 EBS

The default 10GB storage of Cloud9 workspace is quite small. Thus, it is good for us to enlarge the EBS volume used by the Cloud9 instance.

To increase the EBS volume, please perfrom as follows:
1. Select the Cloud9 instance in the EC2 console [Link](https://console.aws.amazon.com/ec2/v2/home#Instances).
2. Click the "Storage" chart
3. Roll down, explore, and click on the "Volume ID" in the list

![Image](../../src/img/Setup/Cloud9-2.jpg)

4. Select the Volume in the list
5. Click "Modify volume" in the sub-menu of the "Actions" button

![Image](../../src/img/Setup/Cloud9-3.jpg)

6. Modify the volume type and size on your own demand (e.g. 100GiB) and click modify.

![Image](../../src/img/Setup/Cloud9-4.jpg)

<div class="code-example" markdown="1">
[Previous Step](http://example.com/){: .btn }
[Next Step](http://example.com/){: .btn .btn-purple }
</div>
