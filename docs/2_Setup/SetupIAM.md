---
layout: default
title: Setup AWS IAM
parent: Setup
nav_order: 1
---

# IAM
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---
## Create an IAM role for your Workspace

Head over to the [IAM console](https://console.aws.amazon.com/iam/home), find and click "Create role" button **(2)** under the Roles **(1)** section.

![Image](../../src/img/Setup/Cloud9-5.jpg)

Select the "AWS service" **(3)** and choose the "EC2" use case **(4)** hit Next button **(5)** at the bottom.

![Image](../../src/img/Setup/Cloud9-6.jpg)

Select the policy named "AdministratorAccess" **(6)**.

![Image](../../src/img/Setup/Cloud9-7.jpg)

Tag the role with a key named "aws-workshop" with an empty value, name the role as "aws-workshop-admin" and click "Create role".

![Image](../../src/img/Setup/Cloud9-8.jpg)

![Image](../../src/img/Setup/Cloud9-9.jpg)

Therefore, a role named "aws-workshop-admin" is ready for use.

<div class="code-example" markdown="1">
[Previous Step](https://juychen.github.io/docs/2_Setup/Setup.html){: .btn }
[Next Step](https://juychen.github.io/docs/2_Nextflow/Nextflow.html){: .btn .btn-purple }
</div>