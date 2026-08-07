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

## 2026-07-28

### Amazon S3

Continued studying Amazon S3 by focusing on bucket architecture, object management, metadata, checksums, and object change detection using Terraform.

### Topics Covered

* Learned S3 bucket naming rules and service limitations
* Compared general purpose and directory buckets
* Explored virtual folders and object prefixes
* Studied ETags and their role in detecting content changes
* Learned how Terraform uses `filemd5()` to track object updates
* Explored checksum algorithms used for data integrity
* Learned the difference between system-defined and user-defined metadata
* Uploaded objects with custom metadata using the AWS CLI

### Commands Practiced

```bash
aws s3 mb
aws s3 cp
aws s3api head-object
aws s3api put-object
md5sum
terraform plan
terraform apply --auto-approve
terraform destroy
```

### What I Learned

* Buckets are infrastructure, while objects represent the actual stored data.
* S3 folders are virtual prefixes rather than real directories.
* ETags can be used to detect changes to object contents without downloading the object.
* Checksums verify data integrity during uploads and downloads.
* Metadata provides descriptive information about an object without changing its contents.
* Terraform can detect object changes using file hashes, making object updates manageable through Infrastructure as Code.

### Repository Updates

* Expanded the Amazon S3 documentation with bucket and object concepts.
* Added notes covering ETags, checksums, metadata, prefixes, and bucket limitations.
* Documented additional AWS CLI and Terraform examples.

## 2026-07-29

### Amazon S3 (Bucket Features & Storage Classes)

Continued exploring Amazon S3 by studying bucket features, object protection, request styles, and the different storage classes available for various workloads.

### Topics Covered

- WORM (Write Once Read Many)
- S3 Object Lock
- Bucket URIs
- AWS S3 command families (`aws s3`, `aws s3api`, `aws s3control`)
- Virtual-hosted vs Path-style requests
- Dualstack endpoints
- S3 Standard
- Standard-IA
- One Zone-IA
- Express One Zone
- Glacier storage classes
- Reduced Redundancy Storage (legacy)

### Commands Practiced

```bash
aws configure set s3.addressing_style virtual
```

```bash
aws s3 cp hello.txt s3://bucket-name --storage-class STANDARD_IA
```

```bash
aws s3api put-object \
--bucket bucket-name \
--key hello.txt \
--body hello.txt \
--object-lock-mode GOVERNANCE \
--object-lock-retain-until-date "2027-01-01T00:00:00Z"
```

### What I Learned

- WORM makes objects immutable for compliance and auditing.
- Object Lock must be enabled when the bucket is created.
- Different S3 command families serve different levels of abstraction.
- Virtual-hosted style requests are replacing path-style requests.
- Choosing the correct storage class is a trade-off between cost, availability, durability, and retrieval speed.

## 2026-07-31

### Amazon S3 (Storage Classes & Security)

Continued studying Amazon S3 by exploring the remaining storage classes and the security mechanisms used to control access to buckets and objects.

### Topics Covered

- Glacier Flexible Retrieval
- Glacier Deep Archive
- Intelligent-Tiering
- S3 Security Overview
- Block Public Access
- Access Control Lists (ACLs)
- Bucket Policies
- IAM Policies
- S3 Access Grants

### Commands Practiced

```bash
aws s3api put-object --storage-class INTELLIGENT_TIERING
```

```bash
aws s3api put-public-access-block
```

```bash
aws s3api get-public-access-block
```

```bash
aws s3api put-bucket-acl
```

```bash
aws s3api put-bucket-ownership-controls
```

### What I Learned

- Glacier storage classes trade retrieval speed for lower storage costs.
- Intelligent-Tiering automatically optimizes storage costs based on object access.
- Block Public Access is one of the most important S3 security features.
- ACLs are considered legacy, while Bucket Policies and Access Points are the preferred access-control mechanisms.
- Bucket Policies and IAM Policies solve similar problems but operate on different types of AWS resources.

## 2026-08-01

### S3 Security and CORS (Session 2)

Continued studying Amazon S3 security and networking features.

#### Topics Covered

- IAM Access Analyzer for S3
- Internetwork Traffic Privacy
- AWS PrivateLink
- VPC Gateway Endpoints
- Cross-Origin Resource Sharing (CORS)
- Static Website Hosting
- Bucket Policies
- Browser Same-Origin Policy
- API Gateway Mock Integration
- CORS configuration for S3 buckets

#### Practical Work

- Created multiple static website buckets
- Configured Bucket Policies
- Modified Block Public Access settings
- Enabled Static Website Hosting
- Uploaded HTML and JavaScript assets
- Retrieved website endpoints
- Created an API Gateway mock endpoint
- Tested cross-origin browser requests
- Configured S3 CORS rules
- Observed browser behavior with and without CORS enabled

#### What I Learned

- Public buckets are not automatically accessible from browsers due to the Same-Origin Policy.
- CORS is enforced by web browsers rather than Amazon S3 itself.
- AWS PrivateLink and Gateway Endpoints provide private connectivity without using the public Internet.
- IAM Access Analyzer helps detect unintentionally exposed S3 buckets.
- Static website hosting, bucket policies, and CORS are commonly used together when serving web applications from S3.

### S3 Encryption

Continued learning Amazon S3 security by studying encryption mechanisms and key management.

#### Topics Covered

- Encryption in Transit
- Encryption at Rest
- Client-Side Encryption (CSE)
- Server-Side Encryption (SSE)
- SSE-S3
- SSE-KMS
- SSE-C
- DSSE-KMS
- CORS configuration and cleanup

#### Practical Work

- Applied CORS rules using JSON configuration.
- Reviewed cleanup procedures for API Gateway and S3 resources.
- Explored uploading objects using different encryption methods.
- Compared AWS-managed keys, KMS-managed keys, and customer-managed keys.
- Learned how AWS KMS integrates with Amazon S3.

#### What I Learned

- TLS protects data while it travels over networks, while SSE and CSE protect stored data.
- SSE-S3 is enabled by default for all new S3 objects.
- SSE-KMS provides stronger control, auditing, and compliance through AWS KMS.
- SSE-C gives customers full responsibility for encryption keys.
- DSSE-KMS applies two layers of encryption for highly sensitive data.

## 2026-08-03

### S3 Encryption

Completed studying Amazon S3 encryption options.

Covered:

- Encryption in Transit (TLS/SSL)
- Encryption at Rest
- SSE-S3
- SSE-KMS
- SSE-C
- DSSE-KMS
- Bucket Keys
- Practical AWS CLI examples for each encryption method
- KMS permissions and pricing considerations
- Bucket-level encryption configuration

## 2026-08-04

## Completed

- Learned Client-Side Encryption (CSE)
- Understood Strong vs Eventual Consistency
- Learned S3 Replication
  - Cross-Region Replication (CRR)
  - Same-Region Replication (SRR)
  - Bi-Directional Replication
  - Batch Replication
- Learned S3 Versioning
- Learned S3 Lifecycle
- Learned S3 Transfer Acceleration
- Enabled Transfer Acceleration using the AWS CLI
- Configured the AWS CLI to use Virtual Hosted-Style requests
- Uploaded objects through the Transfer Acceleration endpoint

## Next Topics

- S3 Object Ownership
- Object Tags
- MFA Delete
- Access Points
- Remaining S3 Security Features

## 2026-08-05

## Completed

- Learned how Presigned URLs provide temporary access to private S3 objects.
- Understood the anatomy of a Presigned URL and its authentication parameters.
- Learned how S3 Access Points simplify bucket permission management.
- Learned Internet and VPC Access Points.
- Understood Access Point Policies.
- Learned Multi-Region Access Points.
- Learned how AWS Global Accelerator routes requests to the nearest bucket.
- Learned Object Lambda Access Points.
- Learned how Lambda transforms S3 objects without modifying the original data.
- Installed and used Mountpoint for Amazon S3.
- Learned supported and unsupported Mountpoint operations.
- Learned Archive Storage Classes.
- Learned Intelligent-Tiering Archive Access tiers.
- Compared manual archival with automatic archival.

## Next Topics

- Requester Pays
- AWS Marketplace for S3
- Batch Operations
- S3 Inventory
- S3 Select

## 2026-08-05

## Completed

- Object Lambda Access Points
- Mountpoint for Amazon S3
- Archived Objects
- Requester Pays
- AWS Marketplace for S3
- Batch Operations
- S3 Inventory
- S3 Select

## 2026-08-06

## Completed

### Monitoring & Automation

- S3 Event Notifications
- S3 Storage Class Analysis
- Amazon S3 Storage Lens

## 2026-08-06

## ✅✅✅ Completed

### Advanced Amazon S3

- Static Website Hosting
- Multipart Upload
- Byte Range Fetching
- S3 Interoperability

---

## Amazon S3 Completed

### Core

- Buckets
- Objects
- Object Lock (WORM)
- Storage Classes
- Glacier Storage Classes
- Intelligent-Tiering

### Security

- Bucket Policies
- ACLs
- Access Grants
- Access Analyzer
- Block Public Access
- PrivateLink
- Gateway Endpoints
- CORS
- Encryption
- SSE-S3
- SSE-KMS
- SSE-C
- DSSE-KMS
- Client-Side Encryption

### Data Management

- Consistency
- Replication
- Versioning
- Lifecycle
- Transfer Acceleration
- Presigned URLs
- Access Points
- Multi-Region Access Points
- Object Lambda
- Mountpoint for S3
- Archived Objects
- Requester Pays
- Batch Operations
- Inventory
- S3 Select

### Monitoring & Analytics

- Event Notifications
- Storage Class Analysis
- Storage Lens

### Advanced Features

- Static Website Hosting
- Multipart Upload
- Byte Range Fetching
- S3 Interoperability

## 2026-08-07

## Completed

### AWS API

- What is an API?
- HTTP/S APIs
- AWS Service Endpoints
- AWS API Request Structure
- Authentication & Signed Requests
- API Actions & Parameters
- Ways to Interact with AWS APIs
- HTTP Requests
- AWS Management Console
- AWS SDKs
- AWS CLI

---

## AWS API Completed

### Core

- Application Programming Interface (API)
- HTTP/S Requests
- AWS Service Endpoints
- Request Structure
- API Actions
- Request Parameters
- Request Payloads
- Authentication
- Signed Requests

### AWS CLI

- Command Line Interface
- Terminal
- Console
- Shell
- Bash
- Zsh
- PowerShell
- AWS CLI
- CLI Commands
- CLI Output Formats
- AWS CLI Installation

### Credentials

- Access Keys
- Access Key ID
- Secret Access Key
- Programmatic Access
- AWS Console Access vs Programmatic Access
- AWS Credentials
- AWS Credentials File
- `~/.aws/credentials`
- AWS CLI Profiles
- `aws configure`
- Environment Variables
- Credential Security Best Practices
- Access Key Rotation
- Access Key Activation/Deactivation
- Two Active Access Keys Limit

### API Reliability

- Network Failures
- DNS Failures
- Network Devices
- Load Balancers
- API Retries
- Exponential Backoff
- Retry Intervals
- CLI/SDK Built-in Retries

### Smithy

- Smithy
- Smithy 2.0
- AWS Interface Definition Language (IDL)
- Model-First Development
- Service Definitions
- Operations
- Structures
- Lists