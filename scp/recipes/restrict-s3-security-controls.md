# SCP for S3

## Restrict S3 buckets with public access

Use the following SCP to restrict the creation/modification of S3 buckets with public access:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": [
        "s3:CreateBucket",
        "s3:PutBucketAcl",
        "s3:PutBucketPolicy",
        "s3:PutObject",
        "s3:PutObjectAcl"
      ],
      "Resource": "arn:aws:s3:::*",
      "Condition": {
        "StringNotEquals": {
          "s3:x-amz-acl": "private"
        }
      }
    }
  ]
}
```

## Restrict HTTPS for S3 buckets

Use the following SCP to restrict the creation/modification of S3 buckets without HTTPS:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": ["s3:CreateBucket", "s3:PutBucketPolicy"],
      "Resource": "arn:aws:s3:::*",
      "Condition": {
        "Bool": {
          "aws:SecureTransport": "false"
        }
      }
    }
  ]
}
```

## Enforce encryption for S3 buckets

Use the following SCP to deny the creation or modification of S3 buckets unless encryption is enforced on the bucket. This SCP ensures that any new bucket created or any bucket being modified must have encryption enabled (server-side encryption using either S3-managed keys AES256 or AWS KMS).

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": [
        "s3:CreateBucket",
        "s3:PutBucketPolicy",
        "s3:PutEncryptionConfiguration"
      ],
      "Resource": "arn:aws:s3:::*",
      "Condition": {
        "StringNotEquals": {
          "s3:x-amz-server-side-encryption": ["AES256", "aws:kms"]
        }
      }
    },
    {
      "Effect": "Deny",
      "Action": "s3:PutObject",
      "Resource": "arn:aws:s3:::*/*",
      "Condition": {
        "StringNotEquals": {
          "s3:x-amz-server-side-encryption": ["AES256", "aws:kms"]
        }
      }
    }
  ]
}
```
