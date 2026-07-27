# AWS SAA-C03 Learning Progress

## 2026-07-24

### AWS Environment Setup

Completed the initial AWS environment setup:

- Created AWS account
- Enabled MFA for the root user
- Created a Zero Spend Budget
- Created an IAM administrator user
- Created AWS CLI access keys
- Configured AWS CLI locally
- Verified AWS CLI authentication

### Commands Practiced

```bash
aws configure
```

```bash
aws sts get-caller-identity
```

### Notes
### What I learned

- IAM users are preferred over using the root account for daily AWS tasks.
- AWS CLI allows managing AWS resources from the terminal.
- Budgets help monitor unexpected AWS spending.

Started documenting my journey while preparing for the AWS Certified Solutions Architect – Associate (SAA-C03) certification.

## 2026-07-25

### Amazon S3

Completed my introduction to Amazon S3 through both the AWS Management Console and the AWS CLI.

### Topics Covered

* Created and configured an S3 bucket
* Uploaded objects through the AWS Management Console
* Learned about bucket versioning, server-side encryption, tags, metadata, and object lock
* Learned how S3 stores objects using prefixes instead of traditional folders
* Explored S3 storage classes
* Used AWS CloudShell to interact with S3
* Practiced high-level `aws s3` commands
* Practiced low-level `aws s3api` commands
* Learned the difference between `aws s3` and `aws s3api`

### Commands Practiced

```bash
aws s3
aws s3 ls
aws s3 cp
aws s3 mv
aws s3 rm
aws s3 sync
aws s3 mb
aws s3 rb
```

```bash
aws s3api create-bucket
aws s3api list-buckets
aws s3api copy-object
aws s3api get-object
aws s3api put-object
```

### What I Learned

* Amazon S3 is an object storage service that stores data as objects inside buckets.
* Buckets must be empty before they can be deleted.
* The folders shown in the AWS Console are actually object key prefixes, not real directories.
* The `aws s3` commands provide high-level file operations, while `aws s3api` provides lower-level access to the S3 API.
* AWS CloudShell provides a Linux environment with the AWS CLI already installed.

### Repository Updates

* Added the `01-Storage/S3` section to this repository.
* Documented Amazon S3 concepts in `README.md`.
* Created a dedicated CLI reference (`cli.md`).
* Recorded additional concepts and observations in `notes.md`.

## 2026-07-25 (Continued)

### Amazon S3

Continued learning Amazon S3 by exploring more advanced AWS CLI commands and introducing Bash scripting to automate common S3 operations.

### Topics Covered

* Listed objects using `aws s3api`
* Filtered JSON responses using JMESPath queries
* Learned how object prefixes represent folders in S3
* Practiced working with object keys
* Introduced Bash scripting for AWS automation
* Created reusable scripts for bucket and object management
* Learned how to make Bash scripts executable using `chmod`
* Used shebangs to create portable Bash scripts
* Learned input validation for Bash scripts
* Learned how `LocationConstraint` is required when creating buckets outside `us-east-1`
* Used `jq` to sort and filter JSON output
* Explored Linux utilities such as `tree`, `dd`, and `set -e`

### Commands Practiced

```bash
aws s3api list-objects
aws s3api list-objects-v2
chmod
tree
jq
```

### What I Learned

* Backslashes (`\`) allow long Bash commands to span multiple lines for better readability.
* `aws s3api` provides lower-level access to Amazon S3 than the high-level `aws s3` commands.
* Reusable Bash scripts make AWS operations repeatable and are a step toward Infrastructure as Code (IaC).
* The recommended portable shebang is `#!/usr/bin/env bash`.
* Bash scripts should validate user input before executing AWS CLI commands.
* `jq` is a powerful tool for filtering and formatting JSON returned by the AWS CLI.
* Buckets created outside `us-east-1` require a `LocationConstraint` when using the low-level S3 API.

### Repository Updates

* Expanded the Amazon S3 documentation.
* Added advanced AWS CLI examples.
* Documented Bash scripting concepts and Linux utilities used with AWS CloudShell.

## 2026-07-26 (Session 3)

### Amazon S3

Continued building on Amazon S3 by exploring automation through Bash and PowerShell, using the AWS SDK for Ruby, and introducing Infrastructure as Code with AWS CloudFormation.

### Topics Covered

* Automated S3 tasks with reusable Bash scripts
* Created a script to delete all objects in a bucket
* Learned how to use `jq` to process AWS CLI JSON output
* Explored AWS Tools for PowerShell
* Created and uploaded files using PowerShell cmdlets
* Learned the basics of the AWS SDK for Ruby
* Used Ruby gems to interact with Amazon S3
* Introduced CloudFormation for Infrastructure as Code
* Learned about CloudFormation deployments and stack deletion
* Briefly explored Terraform as a multi-cloud IaC solution

### Commands Practiced

```bash
aws s3api delete-objects
aws cloudformation deploy
aws cloudformation delete-stack
bundle init
bundle install
bundle exec ruby s3.rb
```

### What I Learned

* Reusable scripts save time and reduce repetitive AWS CLI commands.
* PowerShell uses a Verb-Noun naming convention for AWS cmdlets.
* SDKs allow applications to interact with AWS services programmatically.
* CloudFormation templates define infrastructure as code and support idempotent deployments.
* Terraform is a widely used third-party Infrastructure as Code tool that supports multiple cloud providers.

### Repository Updates

* Expanded the Amazon S3 documentation with additional CLI examples.
* Added notes covering Bash scripting, PowerShell, the AWS SDK for Ruby, and CloudFormation.
* Continued organizing the repository to document both concepts and practical examples.

## 2026-07-27 (Session 4)

### Amazon S3

Wrapped up the Amazon S3 section by exploring several Infrastructure as Code (IaC) tools capable of provisioning S3 resources using code instead of manual configuration.

### Topics Covered

* Introduced Terraform and its workflow
* Learned about Terraform providers, resources, and state files
* Explored AWS CDK using TypeScript
* Learned the difference between the AWS SDK and AWS CDK
* Briefly introduced Pulumi as another multi-cloud IaC solution

### Commands Practiced

```bash
terraform init
terraform apply
terraform destroy
cdk bootstrap
cdk synth
cdk deploy
pulumi up
```

### What I Learned

* Terraform uses providers and resources to manage cloud infrastructure.
* The `terraform.tfstate` file stores the current infrastructure state and should be protected because it may contain sensitive information.
* AWS CDK allows infrastructure to be written in programming languages while generating CloudFormation templates behind the scenes.
* AWS SDKs are intended for application development, whereas AWS CDK is designed specifically for Infrastructure as Code.
* Pulumi is another Infrastructure as Code platform that supports multiple cloud providers and programming languages.

### Repository Updates

* Expanded the Amazon S3 documentation with notes on Terraform, AWS CDK, and Pulumi.
* Continued documenting different approaches to Infrastructure as Code for AWS.
