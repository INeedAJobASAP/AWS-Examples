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

# WORM (Write Once Read Many)

- WORM makes data immutable. Once written, an object cannot be modified or deleted until its retention expires.
- Commonly used for regulatory compliance, auditing, healthcare, finance, and legal records.
- Objects remain readable even while protected.

---

# S3 Object Lock

- Object Lock implements the WORM model for S3 objects.
- Must be enabled **when the bucket is created** and cannot be enabled later.
- Protects objects from accidental or intentional deletion.
- Supports two protection methods:
  - **Retention Period** – locks an object until a specific date.
  - **Legal Hold** – locks an object indefinitely until manually removed.
- Object Lock settings are configured through the AWS API (CLI or SDK), not the AWS Management Console.
- Buckets with Object Lock enabled cannot be used as destinations for Server Access Logging.

---

# S3 URI

- S3 resources are referenced using the `s3://` URI format.
- Format:

```text
s3://bucket-name/object-key
```

- Used extensively by AWS CLI commands such as `cp`, `sync`, `mv`, and `rm`.

---

# AWS S3 Command Families

### aws s3

- High-level interface.
- Simplifies everyday bucket and object operations.
- Best choice for most administrative tasks.

### aws s3api

- Low-level interface.
- Maps closely to the S3 API.
- Returns structured JSON responses.
- Provides access to advanced configuration options.

### aws s3control

- Used for advanced S3 administration.
- Common use cases include:
  - Access Points
  - Batch Operations
  - Storage Lens
  - S3 Outposts management

### aws s3outposts

- Only used when managing Amazon S3 on AWS Outposts.

---

# Request Styles

There are two ways REST requests reference an S3 bucket.

### Virtual Hosted Style

```
bucket-name.s3.region.amazonaws.com
```

- Preferred method.
- AWS is moving toward this style.

### Path Style

```
s3.region.amazonaws.com/bucket-name
```

- Legacy request style.
- Being deprecated.

---

# Dualstack Endpoints

Amazon S3 supports two endpoint types.

Standard Endpoint

- IPv4 only.

Dualstack Endpoint

- Supports both IPv4 and IPv6.
- Designed to support the transition toward IPv6.

---

# S3 Storage Classes

Storage classes trade off:

- Cost
- Availability
- Durability
- Retrieval speed
- Retrieval cost

Always choose the storage class based on how frequently data will be accessed.

---

# S3 Standard

- Default storage class.
- Multi-AZ.
- 11 nines durability.
- 99.99% availability.
- Millisecond access.
- No retrieval fees.
- Best for frequently accessed data.

---

# S3 Standard-IA

- Multi-AZ.
- Intended for infrequently accessed data.
- Retrieval fee applies.
- 30-day minimum storage duration.
- Good for backups and disaster recovery.

---

# S3 One Zone-IA

- Stores data in a single Availability Zone.
- Lower availability than Standard-IA.
- Cheapest IA storage class.
- Suitable only for data that can be recreated.

---

# S3 Express One Zone

- Uses Directory Buckets.
- Lowest latency S3 storage class.
- Up to 10× faster data access than Standard.
- Designed for latency-sensitive workloads.
- Supports only a single Availability Zone.

---

# S3 Glacier Storage Classes

### Glacier Instant Retrieval

- Immediate retrieval.
- Lowest-cost option for data that still requires instant access.
- 90-day minimum storage duration.

### Glacier Flexible Retrieval

- Retrieval takes minutes to hours.
- Designed for long-term archives.

### Glacier Deep Archive

- Lowest storage cost.
- Retrieval may take around 12 hours.
- Intended for data that is rarely accessed.

---

# Reduced Redundancy Storage (RRS)

- Legacy storage class.
- Originally introduced before Standard storage became less expensive.
- No longer recommended for new workloads.

# Glacier Flexible Retrieval

- Formerly called Amazon S3 Glacier.
- Supports Expedited, Standard, and Bulk retrieval.
- Retrieval cost is separate from storage cost.
- Archived objects include approximately 40 KB of metadata.
- Better to archive fewer large files than many small files.
- Does not require Glacier Vaults.

---

# Glacier Deep Archive

- Lowest-cost S3 storage class.
- Retrieval takes 12–48 hours.
- No expedited retrieval option.
- Designed for compliance and long-term archival.

---

# Intelligent-Tiering

- Automatically moves objects based on usage.
- Frequent Access → Infrequent Access → Archive Instant Access.
- Optional Archive Access and Deep Archive Access tiers.
- AWS charges a small monitoring fee.

---

# Block Public Access

- Enabled by default.
- Protects buckets from accidental public exposure.
- One of the most important S3 security features.
- Applies independently to ACLs and Bucket Policies.

---

# Access Control Lists (ACLs)

- Legacy permission model.
- Only grants permissions to AWS accounts.
- Cannot explicitly deny permissions.
- Cannot create conditional access.
- Rarely used in modern AWS environments.
- Bucket Policies and Access Points are generally preferred.

---

# Bucket Policies

- Resource-based policies.
- Applied directly to buckets.
- Ideal for cross-account access.
- Can specify multiple principals.

---

# IAM Policies

- Identity-based policies.
- Attached to IAM users, groups, or roles.
- Can grant access across many AWS services.
- Better when permissions belong to an identity rather than a resource.

---

# Access Grants

- Integrates S3 with enterprise identity providers.
- Supports temporary credentials.
- Provides fine-grained access to bucket prefixes.

---

# Amazon S3 CORS Lab

## IAM Access Analyzer for S3

### What is IAM Access Analyzer?

IAM Access Analyzer for Amazon S3 helps identify buckets that are accessible from:

- The public Internet
- Other AWS accounts

It continuously analyzes bucket permissions and alerts you about potentially risky configurations.

### Requirements

Before Access Analyzer can inspect your S3 buckets, you must first create an **IAM Access Analyzer** for your AWS account.

### Console Features

The S3 console can display:

- Buckets with public access
- Buckets shared with other AWS accounts
- Region selector for reviewing buckets in different regions
- Downloadable security reports

This makes it easier to identify and remediate accidentally exposed buckets.

---

## Internetwork Traffic Privacy

### What is Internetwork Traffic Privacy?

Internetwork Traffic Privacy refers to keeping network traffic private while data travels between AWS services.

Instead of routing traffic across the public Internet, AWS provides private networking options.

### AWS PrivateLink

AWS PrivateLink creates Interface Endpoints (ENIs) inside your VPC.

Features:

- Private connectivity to AWS services
- Supports cross-account communication
- Supports selected Marketplace services
- Supports Endpoint Policies
- Additional cost

### VPC Gateway Endpoints

Gateway Endpoints provide private connectivity to:

- Amazon S3
- Amazon DynamoDB

Characteristics:

- Free
- Traffic stays inside AWS
- Cannot be used cross-account
- Simpler permission model

---

## Cross-Origin Resource Sharing (CORS)

### What is CORS?

Cross-Origin Resource Sharing (CORS) is an HTTP mechanism that allows browsers to request resources from a different origin.

An origin consists of:

- Protocol
- Domain
- Port

Browsers normally block cross-origin requests unless the destination explicitly allows them.

### Common CORS Headers

Request Headers

- Origin
- Access-Control-Request-Method
- Access-Control-Request-Headers

Response Headers

- Access-Control-Allow-Origin
- Access-Control-Allow-Methods
- Access-Control-Allow-Headers
- Access-Control-Allow-Credentials
- Access-Control-Max-Age
- Access-Control-Expose-Headers

---

## Amazon S3 CORS

Amazon S3 supports CORS configuration for buckets, making it possible for web applications hosted on one origin to access resources stored in another origin.

### Supported Formats

CORS rules may be written in:

- JSON
- XML

The AWS Management Console only supports JSON.

### Best Practice

Avoid using:

```json
"AllowedOrigins": ["*"]
```

unless absolutely necessary.

Instead, explicitly list trusted domains.

### Typical Use Cases

- Static websites
- JavaScript applications
- APIs
- Cross-domain uploads
- Loading fonts, images, videos, or scripts from another origin

---

## Lab Overview

In this lab two static websites were created using separate S3 buckets.

Website 1

- Hosted an HTML page

Website 2

- Hosted a JavaScript file

The JavaScript from Website 2 was referenced inside Website 1 to demonstrate browser cross-origin behavior.

An API Gateway mock endpoint was also created to demonstrate CORS requests against an API.

### What was demonstrated

- Static website hosting
- Bucket policies
- Public website hosting
- Browser same-origin policy
- Cross-origin requests
- CORS configuration
- API Gateway mock integrations

Without proper CORS configuration, browsers block requests even if the resource itself is publicly accessible.

---

# Amazon S3 Encryption

## Cleaning Up CORS Lab

After successfully testing CORS:

- Delete the API Gateway.
- Empty both S3 buckets.
- Delete both buckets to avoid unnecessary charges.

---

## Encryption Overview

Amazon S3 protects data using two broad categories of encryption.

### Encryption In Transit

Encryption In Transit protects data while it moves across a network.

Examples include:

- TLS
- SSL (legacy)

The sender encrypts the data before transmission and the receiver decrypts it after it arrives.

Current best practice:

- TLS 1.2
- TLS 1.3

Older SSL versions and TLS 1.0/1.1 are deprecated.

---

### Encryption At Rest

Encryption At Rest protects data while it is stored inside Amazon S3.

There are two approaches:

- Client-Side Encryption (CSE)
- Server-Side Encryption (SSE)

---

## Client-Side Encryption (CSE)

With Client-Side Encryption:

- The client encrypts the data before uploading.
- Amazon S3 stores only encrypted data.
- Amazon S3 never possesses the plaintext encryption key.
- The client is responsible for key storage and rotation.

---

## Server-Side Encryption (SSE)

With Server-Side Encryption:

- Amazon S3 encrypts data after receiving it.
- Amazon S3 decrypts data automatically when authorized users download objects.
- Only the object contents are encrypted; metadata remains unencrypted.

Server-Side Encryption is enabled by default for all new objects.

Encryption options include:

- SSE-S3
- SSE-KMS
- SSE-C
- DSSE-KMS

---

## SSE-S3

SSE-S3 is the default encryption option.

Characteristics:

- AWS fully manages encryption keys.
- Uses AES-256 (AES-GCM).
- Envelope encryption.
- Automatic key rotation.
- No additional cost.
- Bucket Keys may improve performance.

---

## SSE-KMS

SSE-KMS integrates with AWS Key Management Service.

Features:

- Customer chooses the KMS key.
- Automatic key rotation.
- Fine-grained IAM and KMS permissions.
- Compliance support.
- Additional charges apply.
- KMS key must exist in the same AWS Region as the bucket.

Required permissions:

Upload:

- kms:GenerateDataKey

Download:

- kms:Decrypt

---

## SSE-C

SSE-C uses customer-provided encryption keys.

Characteristics:

- Customer supplies the encryption key for every upload and download.
- Amazon S3 never permanently stores the key.
- AWS stores only a salted HMAC for validation.
- Supports presigned URLs.
- Different object versions may use different encryption keys.
- No additional AWS charge.

The customer is entirely responsible for protecting and rotating encryption keys.

---

## DSSE-KMS

Dual-Layer Server-Side Encryption (DSSE-KMS) encrypts data twice.

Workflow:

1. KMS generates a Data Encryption Key (DEK).
2. Client encrypts the data locally.
3. Encrypted DEK is stored alongside the object.
4. During download, KMS decrypts the DEK.
5. Client decrypts the object locally.

Characteristics:

- Two encryption layers.
- Uses AWS KMS.
- Higher security.
- Additional KMS costs.

---

## Choosing an Encryption Method

| Method | Keys Managed By | Cost | Typical Use |
|---------|----------------|------|-------------|
| SSE-S3 | AWS | Free | Default encryption |
| SSE-KMS | Customer (KMS) | Additional charges | Compliance and audit requirements |
| SSE-C | Customer | Free | Full control of encryption keys |
| DSSE-KMS | Customer (KMS) | Highest | Highly sensitive workloads |

# Amazon S3 Encryption

---

## Encryption Overview

Amazon S3 supports two types of encryption:

- Encryption In Transit
- Encryption At Rest

Encryption protects data while it is moving across a network or while it is stored inside Amazon S3.

---

## Encryption In Transit

Encryption In Transit protects data while it travels between a client and Amazon S3.

Common protocols:

- TLS (Transport Layer Security)
- SSL (Secure Socket Layer - Legacy)

Current best practice:

- TLS 1.2
- TLS 1.3

Older SSL versions and TLS 1.0/1.1 are deprecated.

---

## Encryption At Rest

Encryption At Rest protects data stored in Amazon S3.

There are two approaches:

- Client-Side Encryption (CSE)
- Server-Side Encryption (SSE)

---

## Client-Side Encryption (CSE)

With Client-Side Encryption:

- The client encrypts the object before uploading.
- Amazon S3 stores only encrypted data.
- Amazon S3 never has the plaintext encryption key.
- The client is responsible for key storage and rotation.

---

## Server-Side Encryption (SSE)

With Server-Side Encryption:

- Amazon S3 encrypts the object after upload.
- Amazon S3 decrypts the object during download.
- Object contents are encrypted.
- Metadata is not encrypted.

Encryption methods include:

- SSE-S3
- SSE-KMS
- SSE-C
- DSSE-KMS

---

## SSE-S3

SSE-S3 is the default encryption method for every new object uploaded to Amazon S3.

### Features

- AWS manages all encryption keys.
- Uses AES-256 (AES-GCM).
- Automatic key rotation.
- No additional cost.
- Supports Bucket Keys.

---

## SSE-KMS

SSE-KMS uses AWS Key Management Service (KMS).

### Features

- Customer selects the KMS key.
- Automatic key rotation.
- Fine-grained IAM permissions.
- KMS Key Policies.
- Compliance support.
- Additional KMS charges.
- Bucket and KMS key must be in the same Region.

### Required Permissions

Upload:

- kms:GenerateDataKey

Download:

- kms:Decrypt

---

## SSE-C

SSE-C allows customers to provide their own encryption keys.

### Features

- AWS never permanently stores the encryption key.
- Key must be supplied for every upload.
- Key must be supplied for every download.
- Supports presigned URLs.
- Different object versions may use different keys.
- No additional AWS charge.

If the encryption key is lost, the object cannot be recovered.

---

## DSSE-KMS

Dual-Layer Server-Side Encryption (DSSE-KMS) encrypts data twice.

### Workflow

1. AWS KMS generates a Data Encryption Key (DEK).
2. Client encrypts the data.
3. Encrypted DEK is stored with the object.
4. During download, KMS decrypts the DEK.
5. Client decrypts the object.

Provides stronger security than standard SSE-KMS but incurs additional KMS costs.

---

## S3 Bucket Keys

Bucket Keys optimize SSE-KMS.

Normally, Amazon S3 calls AWS KMS for every object request.

Bucket Keys create a temporary bucket-level key that reduces KMS API calls.

### Benefits

- Up to 99% lower KMS request costs.
- Improved performance.
- Reduced KMS API traffic.
- Can be enabled at:
  - Bucket level
  - Object level

Bucket Keys support:

- SSE-S3
- SSE-KMS

# S3 Client-Side Encryption, Replication & Lifecycle Notes

---

# S3 Client-Side Encryption (CSE)

## What is Client-Side Encryption?

Client-Side Encryption (CSE) encrypts data **before** it is uploaded to Amazon S3.

Only the client possesses the encryption key, meaning AWS stores only encrypted data and cannot decrypt it.

### Key Features

- Encryption occurs before upload.
- AWS never has access to the encryption key.
- Highest level of privacy.
- Supported by multiple AWS SDKs.
- You are responsible for key management.

> Think of it as locking a box before shipping it to AWS.

---

## Workflow

```
Plaintext
    │
Encrypt Locally
    │
Encrypted Object
    │
Amazon S3
    │
Download
    │
Decrypt Locally
    │
Plaintext
```

---

# S3 Data Consistency

## What is Data Consistency?

Data consistency determines whether every read returns the latest version of stored data.

---

## Strong Consistency

Every read immediately returns the newest object.

Amazon S3 provides Strong Consistency for:

- PUT
- GET
- DELETE
- LIST

Since January 2020 all S3 operations are strongly consistent.

---

## Eventual Consistency

Older distributed systems may temporarily return stale data until replication completes.

Amazon S3 no longer behaves this way.

---

# S3 Object Replication

Object Replication automatically copies objects between S3 buckets.

Common use cases include:

- Disaster recovery
- Cross-region backups
- Compliance
- Cross-account storage
- Multi-region applications

---

## Replication Types

### Cross-Region Replication (CRR)

Replicates objects to another AWS Region.

Ideal for disaster recovery.

---

### Same-Region Replication (SRR)

Replicates objects within the same Region.

Useful for analytics and log aggregation.

---

### Bi-Directional Replication

Synchronizes two buckets in both directions.

Useful for active-active architectures.

---

### S3 Batch Replication

Replicates existing objects on demand.

Unlike CRR or SRR, this is not continuous.

---

# S3 Versioning

S3 Versioning stores multiple versions of an object.

Instead of overwriting an object, Amazon S3 creates another version.

## Benefits

- Recover deleted objects.
- Restore previous versions.
- Protect against accidental overwrites.
- Required for Replication.
- Integrates with Lifecycle rules.

---

## Bucket States

### Unversioned

Default state.

### Versioning Enabled

New object versions are created automatically.

### Versioning Suspended

Existing versions remain, but new versions are no longer created.

> Versioning cannot be disabled after being enabled. It can only be suspended.

---

# S3 Lifecycle

Lifecycle Rules automatically move or delete objects over time.

## Transition Actions

Automatically move objects to cheaper storage classes.

Examples:

- Standard → Standard-IA
- Standard → Glacier
- Glacier → Deep Archive

---

## Expiration Actions

Automatically delete:

- Current versions
- Previous versions
- Delete markers
- Incomplete multipart uploads

---

## Lifecycle Filters

Lifecycle rules can filter objects by:

- Prefix
- Object Tags
- Minimum Object Size
- Maximum Object Size

Lifecycle works with both current and previous object versions.

---

# S3 Transfer Acceleration

Transfer Acceleration speeds up uploads by routing traffic through CloudFront Edge Locations.

```
User
 │
 ▼
Nearest Edge Location
 │
AWS Global Network
 │
 ▼
S3 Bucket
```

Instead of uploading directly to S3, data first reaches the closest AWS Edge Location.

---

## Requirements

- Bucket must be DNS compliant.
- Bucket name cannot contain periods (.).
- Uses Virtual Hosted-Style requests.
- May take up to 20 minutes after enabling.

---

## Transfer Acceleration Endpoints

Standard

```
https://s3-accelerate.amazonaws.com
```

Dualstack (IPv4 + IPv6)

```
https://s3-accelerate.dualstack.amazonaws.com
```

# S3 Advanced Access Notes

---

# Presigned URLs

## What are Presigned URLs?

A Presigned URL is a temporary URL that grants access to a private S3 object without making the object public.

Presigned URLs can be used to:

- Download private objects.
- Upload objects.
- Share temporary access with users.

You can generate Presigned URLs using:

- AWS CLI
- AWS SDKs

The URL expires automatically after the configured duration.

---

## Anatomy of a Presigned URL

A Presigned URL contains authentication information within its query parameters.

Example:

```
https://mybucket.s3.amazonaws.com/myobject
?X-Amz-Algorithm=AWS4-HMAC-SHA256
&X-Amz-Credential=...
&X-Amz-Date=...
&X-Amz-Expires=300
&X-Amz-SignedHeaders=host
&X-Amz-Signature=...
```

### Important Parameters

**X-Amz-Algorithm**

Signing algorithm used.

Usually:

```
AWS4-HMAC-SHA256
```

---

**X-Amz-Credential**

Contains:

- Access Key
- Date
- Region
- Service
- Signing scope

---

**X-Amz-Date**

Timestamp when the signature was generated.

---

**X-Amz-Expires**

URL expiration time (seconds).

Example:

```
300
```

= 5 minutes.

---

**X-Amz-SignedHeaders**

Lists the headers included in the signing process.

---

**X-Amz-Signature**

The cryptographic signature generated from your AWS Secret Access Key.

---

# S3 Access Points

## What are Access Points?

Access Points provide dedicated endpoints with their own permissions for accessing a shared S3 bucket.

Instead of maintaining one very large Bucket Policy, multiple Access Points can be created for different users, applications, or environments.

Each Access Point has:

- Its own Access Point Policy
- Independent Block Public Access settings
- Independent network controls
- Dedicated endpoint

---

## Network Origins

An Access Point can be configured for:

### Internet

Accessible from the public internet.

### VPC

Accessible only from a specific VPC.

---

## Access Point Policies

Each Access Point can have its own IAM-style policy.

Benefits include:

- Smaller Bucket Policies
- Easier management
- Better separation of permissions
- Different permissions for different applications

---

# Multi-Region Access Points

Multi-Region Access Points provide a single global endpoint that routes requests to the closest healthy S3 bucket.

AWS automatically chooses the bucket with the lowest latency.

## Features

- Global endpoint
- Lowest latency routing
- Uses AWS Global Accelerator
- Supports Internet, VPC and PrivateLink access
- Works with S3 Replication
- Supports bi-directional replication

Ideal for globally distributed applications.

---

# Object Lambda Access Points

Object Lambda Access Points allow Amazon S3 objects to be modified before being returned to clients.

The original object stored in S3 is never modified.

Instead, an AWS Lambda function transforms the response.

Supported operations include:

- GET
- HEAD
- LIST

Common use cases include:

- Image resizing
- Data redaction
- File format conversion
- Dynamic metadata

---

# Mountpoint for Amazon S3

Mountpoint allows an S3 bucket to be mounted as a Linux filesystem.

It is an open-source client optimized for high-throughput workloads.

---

## Supported Operations

- Read existing files
- List files
- Create new files
- Read objects up to 5 TB

---

## Unsupported Operations

- Modify existing files
- Delete directories
- Symbolic links
- File locking

Mountpoint is ideal for applications that need S3 throughput but not full POSIX filesystem functionality.

---

## Supported Storage Classes

- S3 Standard
- S3 Standard-IA
- S3 One Zone-IA
- Reduced Redundancy Storage (Legacy)
- Glacier Instant Retrieval

---

## Unsupported Storage Classes

- Intelligent-Tiering
- Glacier Flexible Retrieval
- Glacier Deep Archive
- Intelligent-Tiering Archive Access
- Intelligent-Tiering Deep Archive Access

---

# Archived Objects

Archived Objects are infrequently accessed objects stored at lower cost in exchange for slower retrieval.

There are two ways to archive data.

---

## Archive Storage Classes

Used when you already know your access pattern.

Examples:

### Glacier Flexible Retrieval

- Minutes to hours

### Glacier Deep Archive

- 12+ hours

Provides the lowest storage costs but requires manually transitioning objects.

---

## Archive Access Tiers

Used when access patterns are unknown.

Amazon S3 automatically moves objects between tiers.

Examples:

### Intelligent-Tiering Archive Access

- Retrieval within minutes

### Intelligent-Tiering Deep Archive Access

- Retrieval within 12+ hours

These tiers cost slightly more than Archive Storage Classes but require no manual intervention.

# S3 Advanced Features

---

## S3 Object Lambda Access Points

S3 Object Lambda Access Points allow you to transform object data as it is returned to clients without modifying the original object.

### Key Features

- Original objects remain unchanged.
- Transformations occur during retrieval.
- Uses AWS Lambda.
- Multiple Object Lambda Access Points can be attached to the same bucket.

### Supported Operations

- GET
- HEAD
- LIST

### Common Use Cases

- Image resizing
- Redacting sensitive information
- Dynamic file formatting
- Content filtering

---

## Mountpoint for Amazon S3

Mountpoint allows an S3 bucket to be mounted as a Linux filesystem.

It is an open-source client optimized for high throughput.

### Supports

- Read existing objects
- Create new files
- List directories
- Files up to 5 TB

### Does NOT Support

- Modifying existing files
- Symbolic links
- File locking
- Directory deletion

### Supported Storage Classes

- Standard
- Standard-IA
- One Zone-IA
- RRS
- Glacier Instant Retrieval

### Unsupported

- Intelligent Tiering
- Glacier Flexible Retrieval
- Glacier Deep Archive
- Archive Access tiers

---

## Archived Objects

Archived objects trade retrieval speed for significantly lower storage cost.

### Archive Storage Classes

Manual archival.

| Storage Class | Retrieval |
|--------------|-----------|
| Glacier Flexible Retrieval | Minutes → Hours |
| Glacier Deep Archive | 12+ Hours |

### Archive Access Tiers

Automatic archival via Intelligent Tiering.

| Tier | Retrieval |
|------|-----------|
| Archive Access | Minutes |
| Deep Archive Access | 12+ Hours |

---

## S3 Requester Pays

Requester Pays shifts download and request costs from the bucket owner to the requester.

### Bucket Owner Pays

- Storage

### Requester Pays

- Download requests
- Data transfer

### Rules

- Authentication required
- Anonymous requests not allowed
- Request must include Requester Pays header

---

## AWS Marketplace for S3

Marketplace solutions integrate directly with S3.

### Categories

#### Backup

- Veeam
- Druva

#### Analytics

- ChaosSearch
- Logz.io
- BryteFlow

#### Monitoring

- Datadog
- Splunk
- Dynatrace

#### Security

- GuardDuty
- Macie
- Trend Cloud One
- Rapid7
- Palo Alto

#### Identity

- IAM
- OneLogin
- FileCloud

---

## S3 Batch Operations

Performs operations across billions of S3 objects.

### Supported Operations

- Copy
- Restore Glacier Objects
- Invoke Lambda
- Replace Tags
- Replace ACLs
- Object Lock
- Legal Hold

### Manifest Formats

- Inventory manifest
- CSV

### Completion Reports

Optional audit reports can be generated.

---

## Amazon S3 Inventory

Creates scheduled reports of bucket contents.

### Frequency

- Daily
- Weekly

### Output Formats

- CSV
- ORC
- Parquet

### Metadata

Can include:

- Size
- Storage Class
- Encryption
- Replication
- Version IDs
- Object Lock
- ACL
- Checksums

Useful for auditing and Batch Operations.

---

## Amazon S3 Select

Query object contents using SQL without downloading the entire object.

### Supported Formats

- CSV
- JSON
- Parquet

### Compression

- GZIP
- BZIP2

### Supported Storage Classes

- Standard
- Standard IA
- One Zone IA
- Intelligent Tiering
- Glacier Instant Retrieval

### Unsupported

- Glacier Flexible Retrieval
- Glacier Deep Archive
- Archive Access tiers
- RRS

Useful for querying very large datasets efficiently.

# S3 Monitoring & Analytics

---

## S3 Event Notifications

Amazon S3 Event Notifications allow buckets to automatically notify other AWS services whenever specific object events occur.

This enables event-driven architectures without polling the bucket.

### Supported Events

- Object Created
- Object Removed
- Restore Completed
- Replication Events
- Object Tagging
- Object ACL Updates
- Lifecycle Expiration
- Lifecycle Transition
- Intelligent-Tiering Archive Events
- Reduced Redundancy Storage (RRS) Object Lost Events

### Destinations

- Amazon SNS
- Amazon SQS (Standard Queues only)
- AWS Lambda
- Amazon EventBridge

> **Note**
>
> FIFO SQS queues are **not supported** directly by S3 Event Notifications.

### Delivery Guarantees

- Delivered **at least once**
- Usually delivered within seconds
- Can occasionally take up to a minute

### Common Use Cases

- Automatically resize uploaded images
- Trigger antivirus scanning
- Process uploaded documents
- Send notifications after uploads
- Start ETL pipelines

---

## S3 Storage Class Analysis

S3 Storage Class Analysis monitors object access patterns and recommends when objects should move from **S3 Standard** to **S3 Standard-IA**.

Unlike Intelligent-Tiering, Storage Class Analysis **does not move objects automatically**. Instead, it provides metrics that can be used together with **Lifecycle Rules**.

### Key Features

- Monitors object access patterns
- Helps reduce storage costs
- Supports up to **1,000 filters** per bucket
- Daily reports
- Export results to CSV
- Export metrics to Amazon QuickSight
- Results become available after **24–48 hours**
- Requires approximately **30 days** of access history for meaningful recommendations

### Why use Storage Class Analysis instead of Intelligent-Tiering?

Storage Class Analysis provides:

- More detailed analytics
- Lower monitoring costs
- Greater control over storage transitions
- Integration with Lifecycle Policies for custom automation

### Limitations

Supports recommendations only between:

- S3 Standard
- S3 Standard-IA

---

## Amazon S3 Storage Lens

Amazon S3 Storage Lens provides storage analytics across your entire AWS Organization.

It helps identify:

- Storage usage
- Cost optimization opportunities
- Data protection improvements
- Access management issues
- Performance trends

### Storage Lens Dashboard

Updated daily and provides:

- Total storage
- Object count
- Fastest-growing buckets
- Fastest-growing prefixes
- Cost trends
- Security recommendations

### Export Options

Reports can be exported as:

- CSV
- Parquet

Metrics can also be exported to:

- Amazon CloudWatch

### Benefits

- Organization-wide visibility
- Identify unused storage
- Optimize storage costs
- Improve security posture
- Improve application performance

# Advanced Amazon S3 Features

---

## S3 Static Website Hosting

Amazon S3 can host static websites consisting of HTML, CSS, JavaScript, images, and other static assets.

### Features

- Hosts static websites directly from an S3 bucket
- Supports:
  - Static Website Hosting
  - Redirect Requests
- Website endpoints are public HTTP endpoints

> **Note**
>
> S3 Website Endpoints **do not support HTTPS**.
>
> To serve HTTPS, place **Amazon CloudFront** in front of the S3 bucket.

### Website Endpoint

Depending on the AWS Region, the endpoint uses either a hyphen or period.

```
http://bucket-name.s3-website-region.amazonaws.com

or

http://bucket-name.s3-website.region.amazonaws.com
```

### Limitations

- HTTP only
- Requester Pays buckets cannot be accessed through Website Endpoints
- Best suited for static content

---

## Amazon S3 Multipart Upload

Multipart Upload allows a single object to be uploaded as multiple independent parts.

AWS recommends Multipart Upload for objects larger than **100 MB**.

### Benefits

- Faster uploads
- Parallel uploads
- Retry failed parts only
- Upload parts in any order
- No expiration after upload initiation
- Upload while generating large files

### Multipart Upload Workflow

1. Initiate Multipart Upload
2. Upload Parts
3. Complete Multipart Upload

During completion S3 combines every uploaded part into one object.

### Multipart Upload Rules

- Maximum **10,000 parts**
- Parts numbered **1–10,000**
- Each uploaded part returns an **ETag**
- Completion requires all ETags

---

## Amazon S3 Byte Range Fetching

Byte Range Fetching allows downloading only a specific portion of an object using the HTTP **Range** header.

This is often referred to as **Multipart Download**.

### Benefits

- Faster downloads
- Parallel downloads
- Better retry performance
- Reduced bandwidth
- Ideal for large objects

Typical range sizes:

- 8 MB
- 16 MB

### Common Use Cases

- Large CSV files
- Log files
- Video streaming
- Large archives
- Database exports

### Workflow

1. Request multiple byte ranges
2. Download each range concurrently
3. Reassemble the file locally

---

## S3 Interoperability

S3 integrates with almost every AWS service and commonly acts as the central storage layer.

### Common AWS Integrations

#### Compute

- Amazon EC2
- AWS Lambda
- Amazon EMR

#### Databases

- Amazon RDS
- Amazon Redshift

#### Analytics

- Amazon Athena
- AWS Glue
- Amazon Kinesis Firehose

#### Monitoring

- AWS CloudTrail
- Amazon CloudWatch Logs

#### Data Pipelines

- AWS Data Pipeline

#### IoT

- AWS IoT Core

### Why S3 is the AWS Data Lake

Many AWS services naturally write their output to S3 because it provides:

- Durable storage
- Low cost
- Scalability
- Integration with analytics services