# AWS APIs

## What is an API?

An **Application Programming Interface (API)** is software that allows two applications or services to communicate with each other.

The most common type of API communication is through **HTTP/HTTPS requests**.

AWS exposes APIs for its services, allowing applications and developer tools to interact with AWS programmatically.

For example, an application can communicate with AWS services by sending HTTPS requests.

---

## AWS Service Endpoints

Each AWS service provides a **service endpoint** that clients use to send API requests.

Example:

```http
GET / HTTP/1.1
Host: monitoring.us-east-1.amazonaws.com
X-Amz-Target: GraniteServiceVersion20100801.GetMetricData
X-Amz-Date: 20180112T092034Z
Authorization: AWS4-HMAC-SHA256 Credential=REDACTED/20180411/...
Content-Type: application/json
Accept: application/json
Content-Encoding: amz-1.0
Content-Length: 45
Connection: keep-alive
```

An AWS API request can contain:

* HTTP method
* Service endpoint
* Headers
* Authentication information
* Action
* Parameters / request payload

---

## Authentication & Signed Requests

AWS API requests must be authenticated and authorized.

AWS commonly uses **AWS Signature Version 4 (SigV4)** to sign API requests.

The request signature is generated using AWS credentials and information about the request.

The signature allows AWS to verify:

* Who made the request
* Which request was signed
* That the request was not modified in transit

> In normal development, you generally don't manually construct and sign AWS HTTP requests. The AWS CLI and SDKs handle this process for you.

---

## Ways to Interact with AWS APIs

There are several ways to interact with AWS APIs.

### HTTP Requests

Directly communicate with AWS service APIs using HTTP/HTTPS requests.

### AWS Management Console

A web-based graphical interface for interacting with AWS services.

### AWS SDK

AWS SDKs allow applications to interact with AWS APIs using programming languages such as:

* Python
* Java
* JavaScript
* Go
* Ruby
* .NET

### AWS CLI

The AWS CLI allows you to interact with AWS APIs from a terminal or shell.

```text
Application
    |
    +---- AWS SDK
    |
    +---- AWS CLI
    |
    +---- HTTP Request
    |
    v
AWS API
    |
    v
AWS Service
```

---

# AWS Command Line Interface (CLI)

## What is a CLI?

A **Command Line Interface (CLI)** processes commands to a computer program using lines of text.

Operating systems commonly provide a shell through which users interact with command-line programs.

---

## Terminal

A **terminal** is a text-based input/output environment used to interact with a computer.

---

## Console

A **console** traditionally refers to a physical interface used to interact with a computer.

In modern usage, the term is also commonly used to describe graphical administration interfaces such as the **AWS Management Console**.

---

## Shell

A **shell** is a command-line program that interprets commands entered by the user.

Popular shells include:

* Bash
* Zsh
* PowerShell

> People commonly use the terms terminal, shell, and console interchangeably, although they technically refer to different concepts.

---

## AWS CLI

The **AWS Command Line Interface (CLI)** allows users to interact with AWS APIs by entering commands into a shell or terminal.

Example:

```bash
aws ec2 describe-instances \
  --filters Name=tag-key,Values=Name \
  --query 'Reservations[*].Instances[*].{Instance:InstanceId,AZ:Placement.AvailabilityZone,Name:Tags[?Key==`Name`]|[0].Value}' \
  --output table
```

The AWS CLI can also format the API response into different output formats such as:

* JSON
* YAML
* Text
* Table

---

## AWS CLI Installation

The AWS CLI is available for:

* Linux/Unix
* macOS
* Windows

The command-line program is:

```bash
aws
```

---

# AWS Access Keys

Access keys provide **programmatic access** to AWS APIs.

An access key consists of:

* Access Key ID
* Secret Access Key

They are commonly referred to as **AWS credentials**.

A principal must have permission to use the AWS resources accessed through those credentials.

---

## Credential Types

### Programmatic Access

Access keys allow applications and developer tools to communicate with AWS APIs.

Used by:

* AWS CLI
* AWS SDKs
* Applications
* Other development tools

### AWS Management Console Access

Console access traditionally uses a username/password-based authentication mechanism.

> Modern AWS environments commonly prefer temporary credentials and federated access rather than long-lived IAM user access keys.

---

## Access Key Best Practices

* Never share access keys.
* Never commit access keys to Git.
* Never hard-code secret keys into application source code.
* Deactivate unused access keys.
* Rotate credentials when appropriate.
* Give credentials only the permissions they require.

> An access key has the permissions associated with the identity using it. If the identity has excessive permissions, compromising the access key can provide excessive access to AWS resources.

---

# AWS Credential Files

The AWS CLI and SDKs can load credentials from:

```text
~/.aws/credentials
```

Example:

```ini
[default]
aws_access_key_id=AKIA...
aws_secret_access_key=...

[exampro]
aws_access_key_id=AKIA...
aws_secret_access_key=...
```

The `default` profile is used when no other profile is explicitly selected.

Additional profiles allow multiple credential configurations to coexist.

---

## AWS CLI Configuration

The `aws configure` command can configure credentials and default settings interactively.

```bash
aws configure
```

It can configure:

* Access Key ID
* Secret Access Key
* Default Region
* Default Output Format

---

# AWS CLI Profiles

Profiles allow you to maintain multiple AWS credential/configuration sets.

Example:

```ini
[default]
aws_access_key_id=...
aws_secret_access_key=...

[exampro]
aws_access_key_id=...
aws_secret_access_key=...
region=ca-central-1
```

You can select a profile with:

```bash
aws s3 ls --profile exampro
```

---

# Environment Variables

AWS CLI and AWS SDKs can also obtain credentials from environment variables.

```bash
export AWS_ACCESS_KEY_ID=...
export AWS_SECRET_ACCESS_KEY=...
export AWS_DEFAULT_REGION=us-west-2
```

This is particularly useful in:

* CI/CD pipelines
* Cloud development environments
* Containers
* Temporary development environments

Environment variables allow credentials to be supplied without creating or modifying a local AWS credentials file.

For local development, credential profiles are often more convenient.

---

# API Retries & Exponential Backoff

Network requests can fail for many reasons.

Potential points of failure include:

* DNS servers
* Routers
* Switches
* Load balancers
* Network connections
* AWS services

Applications should therefore be designed to retry appropriate failed requests.

---

## Exponential Backoff

**Exponential backoff** progressively increases the amount of time between retry attempts.

Example progression:

| Attempt |    Backoff |
| ------- | ---------: |
| 1       |   1 second |
| 2       |  2 seconds |
| 3       |  4 seconds |
| 4       |  8 seconds |
| 5       | 16 seconds |
| 6       | 32 seconds |

The basic progression is:

```text
1 → 2 → 4 → 8 → 16 → 32
```

This prevents an application from immediately sending large numbers of repeated requests to a service experiencing problems.

### AWS CLI & SDKs

AWS SDKs and the AWS CLI provide built-in retry behavior.

Applications can generally configure retry-related behavior rather than implementing every retry mechanism manually.

---

# Smithy

**Smithy** is AWS's open-source **Interface Definition Language (IDL)** for defining web services and their interfaces.

Smithy supports **model-first development**.

Instead of allowing an API to become implicitly defined by implementation code, the service interface is explicitly defined first.

---

## Why Smithy?

Smithy can be used to define:

* Services
* Operations
* Inputs
* Outputs
* Structures
* Lists
* Data types
* Protocol behavior

AWS uses Smithy extensively as part of its service and SDK ecosystem.

---

## Smithy Example

```smithy
namespace com.amazonaws.s3

service SimpleS3 {
    version: "2023-12-21",
    operations: [ListBuckets, PutObject]
}

operation ListBuckets {
    output: ListBucketsOutput
}

structure ListBucketsOutput {
    buckets: BucketList
}

list BucketList {
    member: Bucket
}
```

The model describes the service interface independently from the implementation.

---

## Key Takeaways

* AWS services expose APIs that applications can communicate with.
* AWS APIs commonly use HTTP/HTTPS.
* AWS API requests must be authenticated and authorized.
* AWS SigV4 is commonly used to sign AWS API requests.
* The AWS CLI and SDKs simplify API interaction.
* The AWS CLI is a command-line interface for AWS APIs.
* A terminal is an input/output environment.
* A shell interprets command-line commands.
* Access keys provide programmatic AWS access.
* Never expose or commit access keys.
* AWS supports credential files, profiles, and environment variables.
* APIs can experience network failures and should use retries appropriately.
* Exponential backoff increases the delay between retry attempts.
* Smithy is AWS's open-source IDL for defining service interfaces.
