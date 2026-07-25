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
