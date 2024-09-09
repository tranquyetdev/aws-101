# IAM Permission Boundary

IAM permission boundaries are a way to delegate administrative permissions to users and services without granting full administrative permissions. An IAM permission boundary is an advanced feature for using a managed policy to set the maximum permissions that an identity-based policy can grant to an IAM entity. An entity's permissions are the intersection of the entity's identity-based policies and its permission boundaries.

Boundary Example: Allow maximum permissions to EC2, S3, and RDS. Deny all other services.

```json
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Action": ["ec2:*", "s3:*", "rds:*"],
    "Resource": "*"
  }
}
```

But only grant specific read-only permissions in IAM identity-based policy:

```json
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Action": ["ec2:Describe*", "s3:Get*", "rds:Describe*"],
    "Resource": "*"
  }
}
```
