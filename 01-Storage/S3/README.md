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
