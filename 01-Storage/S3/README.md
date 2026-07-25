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