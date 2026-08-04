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

# Object Lock (WORM)

## What is WORM?

**Write Once Read Many (WORM)** is a storage compliance feature that makes data immutable. Once data is written, it cannot be modified or deleted, but it can be read an unlimited number of times.

Common use cases include:

* Financial records
* Healthcare data
* Legal documents
* Regulatory compliance
* Audit logs

A simple analogy is a video game cartridge. The game data is written once and can be played repeatedly, but the contents cannot be changed.

---

## Amazon S3 Object Lock

Amazon S3 Object Lock prevents objects from being modified or deleted for a specified period of time or indefinitely.

### Important Points

* Must be enabled when the bucket is created.
* Cannot be enabled later.
* Helps satisfy regulatory requirements.
* Supports the WORM model.
* Compliant with regulations such as **SEC 17a-4** and **FINRA**.

### Protection Methods

Objects can be protected using:

* **Retention Periods**
* **Legal Holds**

> **Note:** Object Lock configuration can only be configured through the AWS API (CLI or SDK), not through the AWS Management Console.

---

# S3 URI

S3 resources are commonly referenced using the URI format:

```text
s3://bucket-name/object-name
```

### Example

```text
s3://my-images/photo.jpg
```

This format is used extensively throughout the AWS CLI.

---

# AWS S3 Command Families

AWS provides several command groups for interacting with Amazon S3.

## aws s3

High-level commands for everyday bucket and object operations.

### Common Operations

* Copy
* Sync
* Move
* Delete

---

## aws s3api

Low-level API access.

Provides access to nearly every S3 API operation and returns structured JSON responses.

---

## aws s3control

Used for advanced administrative features such as:

* Access Points
* Batch Operations
* Storage Lens
* S3 Outposts Management

> **Note:** Most day-to-day work does not require `aws s3control`.

---

## aws s3outposts

Used only when working with Amazon S3 on AWS Outposts.

---

# Request Styles

Amazon S3 supports two REST request styles.

## Virtual Hosted Style

```text
bucket-name.s3.us-east-1.amazonaws.com
```

* Recommended request style.
* AWS is moving toward this approach.

---

## Path Style

```text
s3.us-east-1.amazonaws.com/bucket-name
```

* Legacy request style.
* Being deprecated.

### AWS CLI Configuration

AWS CLI can be configured to always use Virtual Hosted Style requests.

---

# Dualstack Endpoints

Amazon S3 provides two endpoint types.

## Standard Endpoint

* IPv4 only

## Dualstack Endpoint

* IPv4
* IPv6

Dualstack endpoints are intended to support the transition toward IPv6.

---

# S3 Storage Classes

Amazon S3 provides multiple storage classes designed around different trade-offs between:

* Cost
* Performance
* Availability
* Durability
* Retrieval Speed

### Storage Classes Covered

* S3 Standard
* S3 Standard-IA
* S3 One Zone-IA
* S3 Intelligent Tiering
* S3 Express One Zone
* S3 Glacier Instant Retrieval
* S3 Glacier Flexible Retrieval
* S3 Glacier Deep Archive
* S3 Reduced Redundancy Storage (Legacy)

---

## S3 Standard

The default storage class.

### Characteristics

* 99.999999999% durability (11 nines)
* 99.99% availability
* Multi-AZ storage
* Millisecond retrieval
* No retrieval fees
* No minimum storage duration

### Best For

Frequently accessed data.

---

## S3 Reduced Redundancy Storage (RRS)

Legacy storage class.

Originally introduced as a cheaper alternative to Standard storage.

### Important Note

Today it offers no practical advantage and is **not recommended**.

---

## S3 Standard-Infrequent Access (Standard-IA)

Designed for data that is rarely accessed but still requires fast retrieval.

### Characteristics

* 11 nines durability
* 99.9% availability
* Multi-AZ storage
* Retrieval fee
* 30-day minimum storage duration

### Best For

* Backups
* Disaster recovery
* Long-term storage with occasional access

---

## S3 Express One Zone

A high-performance storage class designed for latency-sensitive workloads.

### Features

* Single-digit millisecond latency
* Up to 10× faster than Standard
* Around 50% lower request cost
* Uses Directory Buckets
* Stores data in a single Availability Zone

---

## S3 One Zone-IA

Lower-cost version of Standard-IA.

### Characteristics

* Single Availability Zone
* Lower availability
* Retrieval fee
* 30-day minimum storage duration

### Best For

Data that can be recreated if lost.

---

# Glacier Storage Classes

Amazon S3 provides several Glacier storage classes for long-term archival.

## Glacier Instant Retrieval

### Characteristics

* Millisecond retrieval
* 90-day minimum storage duration
* Retrieval fee

---

## Glacier Flexible Retrieval

### Characteristics

* Retrieval in minutes to hours
* Suitable for long-term archives

---

## Glacier Deep Archive

### Characteristics

* Lowest storage cost
* Retrieval may take around 12 hours
* Intended for long-term archival storage

> Unlike the older **S3 Glacier Vault** service, Glacier storage classes integrate directly with standard S3 buckets.

# Additional S3 Storage Classes

## S3 Glacier Flexible Retrieval

S3 Glacier Flexible Retrieval (formerly Amazon S3 Glacier) combines Amazon S3 and Glacier into a single API while providing lower-cost archival storage with multiple retrieval options.

### Retrieval Tiers

| Tier | Retrieval Time | Notes |
|------|----------------|------|
| Expedited | 1–5 minutes | Fastest retrieval, limited to 250 MB archives |
| Standard | 3–5 hours | Default retrieval option |
| Bulk | 5–12 hours | Lowest-cost retrieval for very large archives |

### Important Notes

- Retrieval costs are charged separately from storage costs.
- Archived objects include approximately **40 KB** of additional metadata.
- Storing fewer large files is generally more cost-effective than storing many small files.
- Glacier Flexible Retrieval is a storage class and **does not require Glacier Vaults**.

---

## S3 Glacier Deep Archive

Glacier Deep Archive provides Amazon S3's lowest-cost storage option for long-term archival.

### Retrieval Tiers

| Tier | Retrieval Time |
|------|----------------|
| Standard | 12–48 hours |
| Bulk | 12–48 hours |

### Characteristics

- Lowest storage cost
- No expedited retrieval option
- 40 KB of archive metadata per object
- Ideal for compliance archives and long-term backups
- Does not require Glacier Vaults

---

## S3 Intelligent-Tiering

Amazon S3 Intelligent-Tiering automatically moves objects between storage tiers based on access patterns.

### Automatic Tiers

- Frequent Access
- Infrequent Access (after 30 days)
- Archive Instant Access (after 90 days)

### Optional Archive Tiers

- Archive Access
- Deep Archive Access (after 180 days)

### Notes

- AWS automatically monitors object access.
- Monitoring incurs a small monthly fee.
- Useful when future access patterns are unknown.

---

# Amazon S3 Security Overview

Amazon S3 includes multiple security mechanisms for protecting buckets and objects.

Topics covered include:

- Bucket Policies
- Access Control Lists (ACLs)
- Block Public Access
- AWS PrivateLink
- CORS
- IAM Access Analyzer
- Object Ownership
- Access Points
- Access Grants
- Versioning
- MFA Delete
- Object Tags
- Encryption (In-transit, Server-side, Client-side)
- Compliance Validation

---

## Block Public Access

Block Public Access is enabled by default and protects buckets from accidental public exposure.

AWS provides four independent settings:

- Block new ACLs
- Ignore existing ACLs
- Block public bucket policies
- Restrict public bucket policies

One of the most common AWS security misconfigurations is leaving S3 buckets publicly accessible.

---

## Access Control Lists (ACLs)

ACLs are the original S3 permission model.

Characteristics:

- Grant permissions only to AWS accounts.
- Cannot grant conditional permissions.
- Cannot explicitly deny permissions.
- Mainly used for legacy cross-account access.

Today, AWS recommends using **Bucket Policies** or **Access Points** instead.

---

## Bucket Policies vs IAM Policies

### Bucket Policies

- Apply only to one bucket.
- Can specify multiple principals.
- Excellent for cross-account access.
- Controlled by Block Public Access settings.

### IAM Policies

- Apply to IAM users, groups, or roles.
- Can grant permissions across multiple AWS services.
- Can manage permissions for multiple buckets simultaneously.

---

## S3 Access Grants

Amazon S3 Access Grants simplify granting access to S3 data through external identity providers.

Supported identity sources include:

- IAM Identity Center
- Active Directory
- Okta

Access Grants provide temporary credentials and allow fine-grained access to specific prefixes within buckets.

---

# S3 Security and Cross-Origin Access

This section expands on Amazon S3 security features and networking concepts that affect how data is accessed.

## IAM Access Analyzer

IAM Access Analyzer continuously monitors S3 buckets for unintended public or cross-account access. It helps identify risky bucket configurations before they become security incidents.

## Internetwork Traffic Privacy

AWS provides private networking options that allow services to communicate without traversing the public Internet.

Topics covered include:

- AWS PrivateLink
- VPC Gateway Endpoints
- Private communication with Amazon S3

## Cross-Origin Resource Sharing (CORS)

CORS controls which websites are allowed to access resources hosted inside an S3 bucket.

During this section I learned:

- Same-Origin Policy
- Cross-Origin requests
- HTTP CORS headers
- Bucket CORS configuration
- Static website hosting
- API Gateway mock integrations
- Testing browser CORS behavior

## Security Concepts Covered

- IAM Access Analyzer
- Public Access Block
- Bucket Policies
- Static Website Hosting
- CORS
- API Gateway integration
- Private networking
- Cross-account considerations

---

# Amazon S3 Encryption

This section explores how Amazon S3 protects data using encryption during transmission and while stored.

## Topics Covered

- Encryption in Transit
- Encryption at Rest
- Client-Side Encryption (CSE)
- Server-Side Encryption (SSE)
- SSE-S3
- SSE-KMS
- SSE-C
- DSSE-KMS
- CORS configuration cleanup

## What I Learned

- TLS protects data while it travels across networks.
- Server-side encryption is enabled by default for all newly uploaded S3 objects.
- Different server-side encryption methods provide different levels of control over encryption keys.
- AWS KMS provides centralized key management, auditing, and automatic key rotation.
- Customer-provided keys (SSE-C) give complete control over encryption but require the client to manage keys securely.
- Dual-layer encryption (DSSE-KMS) encrypts data twice for highly sensitive workloads.

# S3 Encryption

This example demonstrates the different server-side encryption methods available in Amazon S3 and how to use them with the AWS CLI.

## What you'll learn

- Default S3 encryption (SSE-S3)
- Server-Side Encryption with AWS KMS (SSE-KMS)
- Server-Side Encryption with Customer-Provided Keys (SSE-C)
- Bucket Keys for reducing AWS KMS request costs

---

## Create a Bucket

```bash
aws s3 mb s3://encryption-fun-ab-19292
```

---

## Create a Test File

```bash
echo "Hello World" > hello.txt
```

---

## Upload with Default Encryption (SSE-S3)

Since January 2023, every new object uploaded to S3 is automatically encrypted using SSE-S3.

```bash
aws s3 cp hello.txt s3://encryption-fun-ab-19292
```

---

# SSE-KMS

Create a KMS Customer Managed Key from the AWS Console (or CLI).

> Creating a customer-managed KMS key incurs a monthly charge (around \$1/month).

Upload using your KMS key:

```bash
aws s3api put-object \
  --bucket encryption-fun-ab-19292 \
  --key hello.txt \
  --body hello.txt \
  --server-side-encryption aws:kms \
  --ssekms-key-id YOUR_KMS_KEY_ID
```

If your IAM identity has `kms:GenerateDataKey` and `kms:Decrypt` permissions, uploads and downloads work normally.

---

# SSE-C (Customer Provided Keys)

With SSE-C, AWS never stores your encryption key.

You must provide the key every time you upload or download the object.

## Generate a Base64 Key

```bash
export BASE64_ENCODED_KEY=$(openssl rand 32 | base64)

echo "Key: $BASE64_ENCODED_KEY"
```

Generate its MD5 checksum:

```bash
export MD5_VALUE=$(echo -n "$BASE64_ENCODED_KEY" | base64 --decode | openssl dgst -md5 -binary | base64)

echo "MD5: $MD5_VALUE"
```

Upload:

```bash
aws s3api put-object \
  --bucket encryption-fun-ab-135 \
  --key hello.txt \
  --body hello.txt \
  --sse-customer-algorithm AES256 \
  --sse-customer-key "$BASE64_ENCODED_KEY" \
  --sse-customer-key-md5 "$MD5_VALUE"
```

---

## Using aws s3 with SSE-C

Generate a raw encryption key:

```bash
openssl rand -out ssec.key 32
```

Upload:

```bash
aws s3 cp hello.txt s3://encryption-fun-ab-135/hello.txt \
  --sse-c AES256 \
  --sse-c-key fileb://ssec.key
```

Downloading **without** the key fails:

```bash
aws s3 cp s3://encryption-fun-ab-135/hello.txt hello.txt
```

Downloading **with** the key succeeds:

```bash
aws s3 cp s3://encryption-fun-ab-135/hello.txt hello.txt \
  --sse-c AES256 \
  --sse-c-key fileb://ssec.key
```

---

# S3 Bucket Keys

Bucket Keys reduce the number of AWS KMS API requests when using SSE-KMS.

Benefits:

- Up to 99% lower KMS request costs
- Better performance
- Fewer KMS API calls
- Can be enabled at the bucket or object level

Example:

```bash
aws s3api put-bucket-encryption \
  --bucket mybucket \
  --server-side-encryption-configuration '{
    "Rules": [{
      "ApplyServerSideEncryptionByDefault": {
        "SSEAlgorithm": "aws:kms",
        "KMSMasterKeyID": "alias/your-kms-key-alias"
      },
      "BucketKeyEnabled": true
    }]
}'
```

---

## Cleanup

```bash
aws s3 rm s3://encryption-fun-ab-19292/hello.txt

aws s3 rb s3://encryption-fun-ab-19292
```