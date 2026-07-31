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
---

# Additional S3 API Commands

## Upload an HTML File

Amazon S3 automatically detected the file's content type during upload.

```bash
touch index.html
```

```bash
nano index.html
```

```bash
aws s3api put-object \
  --bucket bucket-name \
  --key index.html \
  --body index.html
```

---

## List Objects

Returns every object inside a bucket.

```bash
aws s3api list-objects --bucket bucket-name
```

---

## Return Object Contents

```bash
aws s3api list-objects --bucket bucket-name --query Contents
```

---

## Return Object Keys Only

```bash
aws s3api list-objects --bucket bucket-name --query "Contents[].Key"
```

When objects are stored inside folders, the returned keys include the full prefix.

Example:

```text
images/
images/photo.jpg
```

---

# Bash Scripting

## Recommended Shebang

```bash
#!/usr/bin/env bash
```

Alternative:

```bash
#!/bin/bash
```

---

## Make Scripts Executable

```bash
chmod u+x s3/bash-scripts/*
```

Verify permissions:

```bash
ls -la s3/bash-scripts/
```

---

## Check Required Input

```bash
if [ -z "$1" ]; then
    echo "No bucket name provided."
    exit 1
fi
```

Assign the first argument:

```bash
BUCKET_NAME=$1
```

---

## Create Bucket Script

```bash
aws s3api create-bucket \
  --bucket $BUCKET_NAME
```

For regions other than `us-east-1`:

```bash
aws s3api create-bucket \
  --bucket $BUCKET_NAME \
  --create-bucket-configuration LocationConstraint=ca-central-1 \
  --query Location \
  --output text
```

---

## Delete Bucket Script

```bash
aws s3api delete-bucket \
  --bucket $BUCKET_NAME \
  --query Location \
  --output text
```

---

## List Objects (v2)

```bash
aws s3api list-objects-v2 \
  --bucket $BUCKET_NAME
```

---

## Exit Immediately on Errors

```bash
set -e
```

---

## Synchronize Files

```bash
aws s3 sync local-directory s3://$BUCKET_NAME/files
```

---

## List Buckets

```bash
aws s3 ls
```

---

## Make a New Script Executable

```bash
chmod u+x s3/bash-scripts/list-buckets
```

---

## Display Bucket Names Sorted by Creation Date

```bash
aws s3api list-buckets | jq -r '.Buckets | sort_by(.CreationDate) | reverse | .[] | .Name'
```

---

## Display Only the Five Most Recent Buckets

```bash
aws s3api list-buckets | jq -r '.Buckets | sort_by(.CreationDate) | reverse | .[0:5] | .[] | .Name'
```

---

# Linux Commands Used

Create an empty file:

```bash
touch index.html
```

Edit a file:

```bash
nano index.html
```

Display a directory tree:

```bash
tree directory-name
```

View the manual for `dd`:

```bash
man dd
```

Install a package (Debian/Ubuntu):

```bash
sudo apt-get install package-name
```
---

# Additional JMESPath Examples

## Return Only the Most Recently Created Bucket

```bash
aws s3api list-buckets \
| jq -r '.Buckets | sort_by(.CreationDate) | reverse | .[0] | .Name'
```

Using `.[0]` returns only the newest bucket.

---

# Delete Every Object in a Bucket

List every object and generate the JSON payload required by `delete-objects`.

```bash
aws s3api list-objects-v2 \
  --bucket $BUCKET_NAME \
  --query "Contents[].Key" \
| jq -n '{Objects: [inputs | .[] | {Key: .}]}' \
> /tmp/delete_objects.json
```

Delete every object.

```bash
aws s3api delete-objects \
  --bucket $BUCKET_NAME \
  --delete file:///tmp/delete_objects.json
```

---

# PowerShell Commands

Import the AWS S3 module.

```powershell
Import-Module AWS.Tools.S3
```

Create a bucket.

```powershell
New-S3Bucket -BucketName $bucketName -Region us-east-1
```

Retrieve a bucket.

```powershell
Get-S3Bucket -BucketName $bucketName
```

Create a file.

```powershell
Set-Content -Path $fileName -Value $fileContent
```

Upload a file.

```powershell
Write-S3Object -BucketName $bucketName -File $fileName -Key $fileName
```

Prompt the user.

```powershell
Read-Host -Prompt "Enter the S3 bucket name"
```

Display output.

```powershell
Write-Host "S3 Bucket: $bucketName"
```

---

# Ruby SDK

Initialize a project.

```bash
bundle init
```

Install dependencies.

```bash
bundle install
```

Run the application.

```bash
bundle exec ruby s3.rb
```

Check versions.

```bash
ruby --version
```

```bash
java -version
```

---

# CloudFormation

Deploy a stack.

```bash
aws cloudformation deploy \
  --template-file template.yaml \
  --region ca-central-1 \
  --stack-name $STACK_NAME
```

Delete a stack.

```bash
aws cloudformation delete-stack \
  --stack-name $STACK_NAME \
  --region us-west-2
```

---

# Linux Commands

Make scripts executable.

```bash
chmod u+x filename
```

Execute a script.

```bash
./script-name
```

---

# Terraform

Initialize a Terraform project.

```bash
terraform init
```

Apply infrastructure.

```bash
terraform apply
```

Destroy infrastructure.

```bash
terraform destroy
```

---

# AWS CDK

Bootstrap the AWS environment.

```bash
cdk bootstrap
```

Generate the CloudFormation template.

```bash
cdk synth
```

Deploy the stack.

```bash
cdk deploy
```

---

# Pulumi

Deploy infrastructure.

```bash
pulumi up
```
---

# Checksums

Create a bucket.

```bash
aws s3 mb s3://checksums-example-bucket
```

Create a file.

```bash
echo "Hello Mars" > myfile.txt
```

Generate an MD5 checksum.

```bash
md5sum myfile.txt
```

Upload the object.

```bash
aws s3 cp myfile.txt s3://checksums-example-bucket
```

Retrieve object metadata.

```bash
aws s3api head-object \
  --bucket checksums-example-bucket \
  --key myfile.txt
```

---

# Metadata

Create a bucket.

```bash
aws s3 mb s3://metadata-example-bucket
```

Create a file.

```bash
echo "Hello Mars" > hello.txt
```

Upload an object with metadata.

```bash
aws s3api put-object \
  --bucket metadata-example-bucket \
  --key hello.txt \
  --body hello.txt \
  --metadata Planet=Mars
```

Retrieve metadata.

```bash
aws s3api head-object \
  --bucket metadata-example-bucket \
  --key hello.txt
```

Delete the object.

```bash
aws s3 rm s3://metadata-example-bucket/hello.txt
```

Delete the bucket.

```bash
aws s3 rb s3://metadata-example-bucket
```

---

# Terraform

Generate an MD5 hash for Terraform change detection.

```hcl
etag = filemd5("myfile.txt")
```

Review planned infrastructure changes.

```bash
terraform plan
```

Apply infrastructure automatically.

```bash
terraform apply --auto-approve
```

Destroy infrastructure.

```bash
terraform destroy
```
## Configure Virtual Hosted Style

```bash
aws configure set s3.addressing_style virtual
```

Configure the AWS CLI to always use virtual-hosted style requests instead of path-style requests.

---

## Upload an Object with Standard-IA Storage Class

```bash
aws s3 cp hello.txt s3://bucket-name --storage-class STANDARD_IA
```

Upload an object directly into the Standard-IA storage class.

---

## Upload an Object with Object Lock

```bash
aws s3api put-object \
--bucket bucket-name \
--key hello.txt \
--body hello.txt \
--object-lock-mode GOVERNANCE \
--object-lock-retain-until-date "2027-01-01T00:00:00Z"
```

Upload an object with Object Lock enabled using **Governance Mode** until the specified retention date.

## Upload using Intelligent-Tiering

```bash
aws s3api put-object \
--bucket my-bucket \
--key myfile \
--body path/to/local/file \
--storage-class INTELLIGENT_TIERING
```

Upload an object using the Intelligent-Tiering storage class.

---

## Disable Block Public Access for ACLs

```bash
aws s3api put-public-access-block \
--bucket bucket-name \
--public-access-block-configuration "BlockPublicAcls=false,IgnorePublicAcls=false,BlockPublicPolicy=true,RestrictPublicBuckets=true"
```

Modify Block Public Access settings.

---

## View Block Public Access Configuration

```bash
aws s3api get-public-access-block \
--bucket bucket-name
```

Display the current Block Public Access configuration.

---

## Configure Object Ownership

```bash
aws s3api put-bucket-ownership-controls \
--bucket bucket-name \
--ownership-controls="Rules=[{ObjectOwnership=BucketOwnerPreferred}]"
```

Configure bucket ownership controls.

---

## Apply a Bucket ACL

```bash
aws s3api put-bucket-acl \
--bucket bucket-name \
--access-control-policy file://policy.json
```

Apply an ACL stored in a JSON policy file.

---

## Upload an Object

```bash
touch bootcamp.txt

aws s3 cp bootcamp.txt s3://bucket-name
```

Upload an object to an S3 bucket.

---

## List Bucket Contents

```bash
aws s3 ls s3://bucket-name
```

List objects stored in a bucket.

---

## Remove an Object

```bash
aws s3 rm s3://bucket-name/bootcamp.txt
```

Delete an object.

---

## Remove a Bucket

```bash
aws s3 rb s3://bucket-name
```

Delete an empty bucket.