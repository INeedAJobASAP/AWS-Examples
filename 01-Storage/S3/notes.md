# Amazon S3 Notes

## Instructor Recommendations

* The AWS Management Console is useful for learning, but the AWS CLI and SDKs are more important for real-world cloud engineering and automation.
* Learn both the console and the CLI, but spend more time becoming comfortable with the CLI.

## S3 Basics

* Amazon S3 is an object storage service.
* Data is stored as **objects** inside **buckets**.
* Buckets are created in a specific AWS Region.
* Bucket names must be globally unique.

## Buckets

When creating a bucket, AWS provides several configuration options:

* Bucket name
* AWS Region
* Block Public Access (enabled by default)
* Bucket Versioning
* Tags
* Default server-side encryption
* Object Lock (Write Once, Read Many)

## Objects

Objects can include:

* File data
* Metadata
* Tags
* Storage class
* Encryption settings

Objects can be uploaded manually through the AWS Management Console or programmatically using the AWS CLI or an SDK.

## S3 Folders

Although the AWS Console displays folders, S3 does **not** have a traditional folder structure.

Folders are actually **prefixes** that are part of an object's key.

Example:

`images/photo.jpg`

The object key is `images/photo.jpg`; there is no real `images` directory.

This is important to remember when interacting with S3 programmatically.

## Storage Classes

* The default storage class is **Standard**.
* Storage classes can be changed after an object has been uploaded.
* Depending on the operation, changing a storage class may create a copy of the object.

## Encryption

* Server-side encryption is enabled by default.
* You can specify your own encryption key if needed.

## Uploading Objects

When uploading an object, you can optionally configure:

* Storage class
* Encryption settings
* Tags
* Metadata
* Checksums

## CloudShell

AWS CloudShell provides a Linux Bash environment with the AWS CLI already installed.

## High-Level vs Low-Level AWS CLI

There are two primary ways to interact with Amazon S3 using the AWS CLI:

### High-level (`aws s3`)

Designed for common file operations such as:

* Uploading
* Downloading
* Copying
* Synchronizing

### Low-level (`aws s3api`)

Provides direct access to the Amazon S3 API and exposes more advanced functionality, including JSON output and additional API operations.

## Things I Learned

* Empty buckets generally don't incur storage charges, but storing objects and using certain features may.
* Buckets must be emptied before they can be deleted.
* The AWS Console makes S3 feel similar to cloud storage services like Google Drive, but internally S3 works very differently.
* Understanding object keys and prefixes is important when working with S3 programmatically.

## Listing Objects

Amazon S3 objects can be listed using `aws s3api`.

JMESPath queries can be used to filter the JSON response.

Object keys include prefixes, so objects stored inside folders appear similar to:

* `images/photo.jpg`
* `images/`

This reinforces that folders in S3 are simply prefixes within the object key.

## Bash Scripting

To avoid repeating commands, reusable Bash scripts can be created.

Scripts were created for:

* Creating buckets
* Deleting buckets
* Listing buckets
* Listing objects
* Uploading objects
* Synchronizing files

### Portable Scripts

The instructor recommended not giving Bash scripts a `.sh` extension.

Instead, use a shebang to specify the interpreter.

Preferred shebang:

```bash
#!/usr/bin/env bash
```

This makes the script more portable across Linux systems.

## Making Scripts Executable

New scripts are not executable by default.

Permissions can be updated with `chmod`.

## Bash Script Input Validation

Before executing AWS CLI commands, scripts should verify that the required command-line arguments were provided.

If an expected argument is missing, the script exits with an error message.

## Working with Regions

When using `aws s3api create-bucket`, buckets created outside of `us-east-1` require a `LocationConstraint`.

The default region for low-level S3 API operations is `us-east-1`.

## Infrastructure as Code

Managing AWS resources through scripts provides repeatability and consistency.

Instead of manually creating resources through the console, infrastructure can be created, modified, and deleted through code.

## Useful Linux Commands

* `tree` displays a directory hierarchy.
* `dd` can be used to generate test files.
* `chmod` changes file permissions.
* `set -e` causes a script to exit immediately if any command fails.

## JSON Processing

The instructor introduced `jq` for filtering and formatting JSON output.

It can be used to sort, filter, and extract data returned by AWS CLI commands.

## Additional AWS CLI Concepts

JMESPath array indexing can return only the most recent bucket.

Example:

* `.[0:5]` returns the five most recent buckets.
* `.[0]` returns only the newest bucket.

## Bash Automation

Additional reusable scripts were created for:

* Deleting every object inside a bucket
* Listing buckets
* Creating buckets
* Deleting buckets
* Synchronizing files

Instead of manually deleting resources through the AWS Console, automation allows the same workflow to be repeated consistently.

## PowerShell

PowerShell uses a Verb-Noun naming convention.

Examples include:

* `New-S3Bucket`
* `Get-S3Bucket`
* `Write-S3Object`
* `Set-Content`

PowerShell scripts can prompt the user for input, create resources, and upload files using AWS cmdlets.

## AWS SDK for Ruby

The Ruby SDK requires external gems such as:

* aws-sdk-s3
* pry
* nokogiri

The SDK allows applications to interact directly with Amazon S3 without calling the AWS CLI.

The instructor also introduced:

* `SecureRandom` for generating unique file contents.
* `binding.pry` as a breakpoint for debugging Ruby applications.

## CloudFormation

CloudFormation templates were written in YAML.

The instructor noted:

* JSON was introduced first but is more difficult to read and maintain.
* YAML is generally preferred because it is easier to write.

CloudFormation deployments are **idempotent**.

Running the same deployment multiple times updates existing infrastructure instead of creating duplicate resources.

## Terraform

Terraform was briefly introduced as a popular third-party Infrastructure as Code tool.

Unlike CloudFormation, Terraform supports multiple cloud providers and is commonly used in multi-cloud environments.

## Important Takeaways

* Infrastructure should be created through code whenever possible.
* Scripts become reusable building blocks for automation.
* SDKs are used by applications, while the AWS CLI is primarily used by administrators and engineers.
* Infrastructure as Code simplifies deployment, updates, and maintenance of cloud resources.

## Terraform

Terraform interacts with AWS through the AWS Provider.

Providers expose resources that can be managed through Terraform configuration files.

The instructor introduced the following concepts:

* Providers
* Resources
* `main.tf`
* Terraform state (`terraform.tfstate`)

The state file keeps track of deployed infrastructure and contains sensitive information, making it an important file that should be handled securely.

Common Terraform workflow:

1. Initialize the project.
2. Review the execution plan.
3. Apply the infrastructure.
4. Destroy the infrastructure when no longer needed.

## AWS CDK

AWS CDK allows developers to define infrastructure using programming languages instead of YAML.

The instructor demonstrated CDK using TypeScript.

A minimal CDK stack was able to create an S3 bucket with only a few lines of code.

Important CDK concepts:

* Stacks
* Constructs
* Bootstrapping
* Synthesizing CloudFormation templates
* Deploying infrastructure

### SDK vs CDK

The instructor highlighted the distinction between the two:

**AWS SDK**

* Used by applications.
* Creates and manages AWS resources programmatically.
* Best suited for application development.

**AWS CDK**

* Used to define and deploy cloud infrastructure.
* Generates CloudFormation templates.
* Best suited for Infrastructure as Code.

## Pulumi

Pulumi is another Infrastructure as Code platform.

Key observations:

* Supports multiple cloud providers.
* Uses familiar programming languages.
* Requires local credentials and cloud configuration.
* Executes deployments from the client machine.

The instructor noted that Pulumi would require additional study beyond this introduction.

## S3 Buckets

An S3 bucket is infrastructure that stores S3 objects.

Although Amazon S3 is a global service, every bucket is created in a specific AWS Region.

### Bucket Naming Rules

Important rules include:

* 3–63 characters
* Lowercase letters only
* Numbers, hyphens (`-`), and periods (`.`) are allowed
* Must start and end with a letter or number
* Cannot contain adjacent periods
* Cannot resemble an IP address
* Must be globally unique within an AWS partition
* Cannot contain uppercase letters, underscores, or spaces

### Bucket Restrictions

Key service limits discussed:

* Default limit of 100 general purpose buckets per account
* Bucket quota can be increased through AWS Support
* Buckets must be emptied before deletion
* Unlimited bucket size
* Unlimited number of objects
* Individual objects can be up to 5 TB
* Multipart uploads are recommended for files larger than 100 MB

## Bucket Types

### General Purpose Buckets

Recommended for nearly every workload.

Characteristics:

* Flat object namespace
* Supports nearly every S3 storage class
* No practical prefix limits

### Directory Buckets

Designed for S3 Express One Zone.

Characteristics:

* Directory hierarchy
* Extremely low latency
* Horizontal directory scaling

## Virtual Folders

Folders displayed in the AWS Console are not actual directories.

They are zero-byte objects ending with a forward slash (`/`) that exist only to organize object prefixes.

## ETags

An ETag identifies a specific version of an object's contents.

Important observations:

* Changes when object contents change
* Metadata changes do not affect the ETag
* May or may not equal the MD5 hash depending on encryption
* Frequently used for cache validation and change detection

Terraform can use `filemd5()` to detect changes in local files before updating S3 objects.

## Checksums

Checksums verify data integrity during uploads and downloads.

Supported algorithms include:

* MD5
* CRC32
* CRC32C
* SHA1
* SHA256

The instructor noted that checksum validation can occasionally be confusing due to encoding differences.

## Object Prefixes

Object prefixes simulate folders inside S3.

Example object key:

```text
/assets/images/photo.jpg
```

Prefix:

```text
/assets/images/
```

Filename:

```text
photo.jpg
```

The entire object key (prefix + filename) cannot exceed 1024 bytes.

## Metadata

Metadata describes the object rather than its contents.

Two categories exist:

### System Metadata

Managed by AWS.

Examples include:

* Content-Type
* Cache-Control
* Content-Encoding
* Content-Language
* Expires

Some values, such as `Content-Type`, can be modified.

### User Metadata

Defined by the user.

Custom metadata keys are stored using the `x-amz-meta-` prefix and can be used by applications for categorization, version tracking, compliance, and other business requirements.
