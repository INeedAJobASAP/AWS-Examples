# Amazon S3 CLI Reference

This document contains the AWS CLI commands I learned while studying Amazon S3.

---

# AWS CLI Auto Prompt

## Enable Auto Prompt

Displays command suggestions and auto-completion while typing AWS CLI commands.

```bash
export AWS_CLI_AUTO_PROMPT=on-partial
```

## Check AWS CLI Environment Variables

Displays AWS CLI environment variables.

```bash
env | grep AWS_CLI
```

## Start the AWS S3 Auto Prompt

Starts the interactive prompt for Amazon S3 commands.

```bash
aws s3
```

---

# Two Ways to Work with Amazon S3

The AWS CLI provides two different interfaces for working with Amazon S3.

## High-Level Commands (`aws s3`)

Designed for common file operations such as uploading, downloading, copying, moving, and synchronizing files.

These commands are simpler and easier to use.

## Low-Level Commands (`aws s3api`)

Provides direct access to the Amazon S3 API.

Use `aws s3api` when you need:

* JSON output
* Advanced bucket configuration
* Fine-grained API operations
* JMESPath queries
* More control over requests

> **Note:** Although `--output` is documented for many AWS CLI commands, the instructor demonstrated that it does not behave as expected with the high-level `aws s3` commands. For structured output (JSON, text, table, YAML), use `aws s3api`.

---

# High-Level S3 Commands (`aws s3`)

## List Buckets

Lists every S3 bucket.

```bash
aws s3 ls
```

---

## Upload a File

Uploads a local file into an S3 bucket.

```bash
aws s3 cp test.txt s3://bucket-name/test.txt
```

The instructor noted that `cp` is commonly used to upload (create) objects in Amazon S3.

---

## Move Objects

Moves an object from one location to another.

```bash
aws s3 mv source destination
```

---

## Synchronize a Directory

Uploads all new or modified files from a local directory.

```bash
aws s3 sync images/ s3://bucket-name
```

---

## Delete an Object

Deletes a single object.

```bash
aws s3 rm s3://bucket-name/file.txt
```

Delete every object inside a bucket.

```bash
aws s3 rm s3://bucket-name --recursive
```

Useful flags:

* `--recursive`
* `--dryrun`
* `--quiet`

---

## Create a Bucket

Creates a bucket.

```bash
aws s3 mb s3://bucket-name
```

---

## Remove a Bucket

Deletes an empty bucket.

```bash
aws s3 rb s3://bucket-name
```

If the bucket contains objects, AWS returns an error indicating that the bucket must be emptied first.

### Correct Bucket Deletion Workflow

Attempt to delete the bucket:

```bash
aws s3 rb s3://bucket-name
```

If AWS reports that the bucket is not empty:

```bash
aws s3 rm s3://bucket-name --recursive
```

Then delete the bucket:

```bash
aws s3 rb s3://bucket-name
```

Verify that it has been removed:

```bash
aws s3 ls
```

---

## Configure Static Website Hosting

```bash
aws s3 website
```

---

## Generate a Pre-Signed URL

```bash
aws s3 presign
```

---

# Low-Level S3 API Commands (`aws s3api`)

These commands communicate directly with the Amazon S3 API and provide additional functionality.

## Create a Bucket

```bash
aws s3api create-bucket --bucket bucket-name --region us-east-1
```

Returns a JSON response.

---

## List Buckets

```bash
aws s3api list-buckets
```

---

## Return Bucket Names Only

```bash
aws s3api list-buckets --query "Buckets[].Name"
```

---

## Return Bucket Names as Plain Text

```bash
aws s3api list-buckets --query "Buckets[].Name" --output text
```

---

## Return Bucket Names as a Table

```bash
aws s3api list-buckets --query "Buckets[].Name" --output table
```

---

## Return Output as YAML

```bash
aws s3api list-buckets --output yaml
```

---

## Filter a Specific Bucket

Returns information about one bucket.

```bash
aws s3api list-buckets --query "Buckets[?Name=='bucket-name']"
```

---

## Return Only the Bucket Name

```bash
aws s3api list-buckets --query "Buckets[?Name=='bucket-name'].Name"
```

---

## Return the Bucket Name as Plain Text

```bash
aws s3api list-buckets --query "Buckets[?Name=='bucket-name'].Name" --output text
```

---

## Copy an Object Already Stored in S3

Copies an object from one S3 location to another.

```bash
aws s3api copy-object --copy-source source-bucket/source-key --bucket destination-bucket --key destination-key
```

---

## Download an Object

Downloads an object from S3.

```bash
aws s3api get-object --bucket bucket-name --key hello.txt world.txt
```

---

## Upload an Object

Uploads a local file using the low-level API.

```bash
aws s3api put-object \
  --bucket bucket-name \
  --key world.txt \
  --content-type text/plain \
  --body world.txt
```

The `--content-type` parameter tells Amazon S3 what type of file is being uploaded.

---

# Linux Commands Used in AWS CloudShell

The following are standard Linux commands executed inside AWS CloudShell. They are **not** AWS CLI commands.

## Create an Empty File

```bash
touch hello.txt
```

---

## Edit a File

```bash
nano hello.txt
```

---

## Create a Directory

```bash
mkdir images
```

---

## Move Files

```bash
mv aaa.jpg images/
```

```bash
mv hello.txt images/
```

---

## List Directory Contents

```bash
ls images/
```
