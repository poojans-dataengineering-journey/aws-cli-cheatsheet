# AWS S3 CLI Commands Cheat Sheet

A comprehensive collection of commonly used AWS S3 CLI commands for managing buckets, files, and access controls.

## Table of Contents
- [Buckets Management](#buckets-management)
- [Upload Files](#upload-files)
- [Download Files](#download-files)
- [Sync Files](#sync-files)
- [Delete Files](#delete-files)
- [Copy Files](#copy-files)
- [Move Files](#move-files)
- [Additional Commands](#additional-commands)
- [Public Access Control](#public-access-control)
- [Pre-Signed URLs](#pre-signed-urls)

## Buckets Management

### List all buckets
```bash
aws s3 ls
```
Lists all S3 buckets in your AWS account.

### Create a bucket
```bash
aws s3 mb s3://<bucket-name>
```
Creates a new S3 bucket with the specified name.

### Delete a bucket
```bash
aws s3 rb s3://<bucket-name>
```
Deletes an empty S3 bucket.

With force option (deletes all contents):
```bash
aws s3 rb s3://<bucket-name> --force
```

### List bucket contents
```bash
aws s3 ls s3://<bucket-name>
```
Lists all objects inside the specified bucket.

## Upload Files

### Single file upload
```bash
aws s3 cp <local-file-path> s3://<bucket-name>/<key>
```

### Directory upload
```bash
aws s3 cp <local-directory-path> s3://<bucket-name> --recursive
```

## Download Files

### Single file download
```bash
aws s3 cp s3://<bucket-name>/<key> <local-file-path>
```

### Directory download
```bash
aws s3 cp s3://<bucket-name> <local-directory-path> --recursive
```

## Sync Files

### Local to bucket sync
```bash
aws s3 sync <local-directory-path> s3://<bucket-name>
```

### Bucket to local sync
```bash
aws s3 sync s3://<bucket-name> <local-directory-path>
```

## Delete Files

### Delete single file
```bash
aws s3 rm s3://<bucket-name>/<key>
```

### Delete folder
```bash
aws s3 rm s3://<bucket-name>/<folder-name> --recursive
```

## Copy Files

### Copy within buckets
```bash
aws s3 cp s3://<source-bucket>/<key> s3://<destination-bucket>/<key>
```

### Copy all files between buckets
```bash
aws s3 cp s3://<source-bucket> s3://<destination-bucket> --recursive
```

## Move Files

### Move single file
```bash
aws s3 mv s3://<source-bucket>/<key> s3://<destination-bucket>/<key>
```

### Move all files
```bash
aws s3 mv s3://<source-bucket> s3://<destination-bucket> --recursive
```

## Additional Commands

### Set bucket policies
```bash
aws s3api put-bucket-policy --bucket <bucket-name> --policy file://<policy-file>.json
```

### Enable versioning
```bash
aws s3api put-bucket-versioning --bucket <bucket-name> --versioning-configuration Status=Enabled
```

### Check versioning status
```bash
aws s3api get-bucket-versioning --bucket <bucket-name>
```

## Public Access Control

### Make file public
```bash
aws s3api put-object-acl --bucket <bucket-name> --key <key> --acl public-read
```

## Pre-Signed URLs

### Generate pre-signed URL
```bash
aws s3 presign s3://<bucket-name>/<key> --expires-in <seconds>
```

## Warning
⚠️ Use these commands carefully, especially when performing delete operations.

## License
This cheat sheet is available under the MIT license. Feel free to modify and distribute it as needed.
