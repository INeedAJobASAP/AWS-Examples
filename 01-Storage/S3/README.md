# Amazon S3

Amazon Simple Storage Service (Amazon S3) is AWS's object storage service. It stores data as **objects** inside **buckets** and is designed to provide scalable, durable, and highly available storage.

## What I learned

During this section, I focused on both the AWS Management Console and the AWS CLI. The instructor emphasized that although the console is useful for learning, understanding the CLI and SDKs is more important for real-world cloud engineering.

## Creating an S3 Bucket

When creating a bucket, AWS allows you to configure several options:

- Bucket name
- AWS Region
- Block Public Access (enabled by default)
- Bucket Versioning
- Tags
- Default server-side encryption
- Object Lock (Write Once, Read Many)

An empty S3 bucket does not incur storage charges, but storing objects and enabling certain features may generate costs.

## Uploading Objects

Objects can be uploaded manually through the console by dragging files into the bucket.

Things I learned:

- S3 stores data as objects rather than files in a traditional filesystem.
- Folders shown in the console are actually **prefixes**, not real directories.
- Objects can have different storage classes.
- Server-side encryption is enabled by default.
- Additional metadata and tags can be assigned during upload.

## Important Notes

- Files larger than approximately 160 GB should be uploaded using the AWS CLI or an SDK.
- Objects are easier to manage through the CLI than through the console.
- Buckets must be emptied before they can be deleted.

## Listing Objects

Using the AWS CLI, objects inside a bucket can be listed in JSON format or filtered using JMESPath queries.

Examples include:

* Listing every object in a bucket
* Returning only object keys
* Filtering specific fields from the response

When objects are uploaded into folders, the returned object keys include their prefixes (for example, `images/photo.jpg`).

## Bash Automation

The instructor introduced Bash scripting to automate common S3 tasks.

Instead of repeatedly typing AWS CLI commands, reusable Bash scripts were created for operations such as:

* Creating buckets
* Deleting buckets
* Listing buckets
* Listing objects
* Uploading objects
* Synchronizing directories

Each script accepts command-line arguments, making them reusable for different buckets and environments.

This approach is similar to Infrastructure as Code (IaC), where cloud resources are managed through code rather than manually using the AWS Management Console.

## Automation and Infrastructure as Code

As the course progressed, the focus shifted from manually managing S3 resources to automating them through scripting, SDKs, and Infrastructure as Code (IaC).

Several approaches were introduced:

* Bash scripting
* PowerShell scripting
* AWS SDK for Ruby
* AWS CloudFormation
* Terraform (brief introduction)

The goal was to demonstrate how cloud resources can be managed programmatically instead of relying on the AWS Management Console.

## Bash Automation

Additional Bash scripts were created to automate repetitive S3 operations such as:

* Deleting every object in a bucket
* Deleting buckets
* Listing buckets
* Listing objects
* Uploading objects
* Synchronizing local directories

These scripts validate user input and can be reused for different AWS environments.

## PowerShell

The course introduced the AWS Tools for PowerShell.

PowerShell cmdlets follow a Verb-Noun naming convention, making commands easy to discover and read.

Examples include:

* `New-S3Bucket`
* `Get-S3Bucket`
* `Write-S3Object`

PowerShell scripts were used to:

* Create S3 buckets
* Check if a bucket already exists
* Generate a text file
* Upload files to Amazon S3

## AWS SDK

The AWS SDK for Ruby was introduced to demonstrate interacting with Amazon S3 directly from application code.

The SDK was used to:

* Read environment variables
* Create an S3 client
* Generate random files
* Upload objects programmatically

## Infrastructure as Code

The course concluded with an introduction to Infrastructure as Code using AWS CloudFormation.

A CloudFormation template was created to provision an S3 bucket.

Deployment and deletion were performed through the AWS CLI.

Terraform was briefly introduced as a popular third-party, multi-cloud Infrastructure as Code solution.

## Additional Infrastructure as Code Tools

The course concluded the Amazon S3 section by introducing several Infrastructure as Code (IaC) tools capable of provisioning S3 resources.

Each tool approaches infrastructure differently, but they all aim to automate cloud deployments.

### Terraform

Terraform is a popular third-party Infrastructure as Code tool that supports multiple cloud providers.

Key concepts introduced:

* Providers
* Resources
* State management
* Initialization
* Applying infrastructure
* Destroying infrastructure

### AWS CDK

The AWS Cloud Development Kit (CDK) allows developers to define cloud infrastructure using familiar programming languages instead of writing YAML or JSON templates.

A simple TypeScript example was used to provision an S3 bucket inside a CDK stack.

The deployment workflow included:

* Bootstrapping the AWS environment
* Synthesizing CloudFormation templates
* Deploying the stack

### Pulumi

Pulumi is another Infrastructure as Code platform that allows cloud infrastructure to be written using programming languages.

Unlike AWS CDK, Pulumi supports multiple cloud providers, making it suitable for multi-cloud environments.

The instructor briefly introduced the Pulumi workflow, while noting that it would require further exploration.

## S3 Buckets and Objects

This section expands on the core concepts of Amazon S3 by exploring bucket architecture, object management, and the metadata associated with stored data.

### S3 Buckets

An S3 bucket is the infrastructure that stores S3 objects.

Topics covered include:

* Bucket naming rules
* Bucket restrictions and service limits
* General purpose buckets
* Directory buckets
* Virtual folders
* Bucket versioning
* Bucket encryption
* Static website hosting

### S3 Objects

Unlike buckets, S3 objects represent the actual data stored in Amazon S3.

Key concepts explored include:

* ETags
* Checksums
* Object prefixes
* Metadata
* Object tags
* Object versioning
* Object locking

### Detecting Object Changes

ETags can be used to detect whether the contents of an object have changed without downloading the object itself.

Terraform can use an object's MD5 hash to determine when an object should be updated during deployment.

### Data Integrity

Amazon S3 automatically validates uploaded data using checksums.

AWS supports multiple checksum algorithms including:

* MD5
* CRC32
* CRC32C
* SHA1
* SHA256

### Metadata

Amazon S3 supports both system-defined and user-defined metadata.

System metadata is managed by AWS, while user-defined metadata allows applications to associate custom information with objects.
