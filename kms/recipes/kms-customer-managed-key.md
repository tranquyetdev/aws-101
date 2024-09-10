# KMS Customer Managed Keys

KMS Customer Managed Keys (CMKs) are encryption keys that you create, own, and manage. These keys provide additional control and security over your data encryption. Here are some key points to understand about KMS CMKs:

## Example 1 - Default Key Policy + IAM Policies

In this example, we have a default key policy that allows the root account full access to the key. Then we create IAM policies to grant access to other users or roles. In this case, you could have different policies for key administrators, key users, and key grantors.

**KMS Key Policy:**

```json
{
  "Version": "2012-10-17",
  "Id": "key-default-1",
  "Statement": [
    {
      "Sid": "Enable IAM User Permissions",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::123456789012:root"
      },
      "Action": "kms:*",
      "Resource": "*"
    }
  ]
}
```

Then you can create IAM policies to grant access to other users or roles. In this case, you could have different policies for key administrators, key users, and key grantors.

**IAM Policy for key administrators:**

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Allow access for Key Administrators",
      "Effect": "Allow",
      "Principal": {
        "AWS": [
          "arn:aws:iam::123456789012:user/admin",
          "arn:aws:iam::123456789012:role/admin"
        ]
      },
      "Action": [
        "kms:Create*",
        "kms:Describe*",
        "kms:Enable*",
        "kms:List*",
        "kms:Put*",
        "kms:Update*",
        "kms:Revoke*",
        "kms:Disable*",
        "kms:Get*",
        "kms:Delete*",
        "kms:ScheduleKeyDeletion",
        "kms:CancelKeyDeletion"
      ],
      "Resource": "*"
    }
  ]
}
```

**IAM Policy for key users:**

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Allow use of the key",
      "Effect": "Allow",
      "Principal": {
        "AWS": [
          "arn:aws:iam::123456789012:user/admin",
          "arn:aws:iam::123456789012:role/admin"
        ]
      },
      "Action": [
        "kms:Encrypt",
        "kms:Decrypt",
        "kms:ReEncrypt*",
        "kms:GenerateDataKey*",
        "kms:DescribeKey"
      ],
      "Resource": "*"
    }
  ]
}
```

**IAM Policy for key grantors:**

Use kms:GrantIsForAWSResource condition to allow attachment of persistent resources. This condition key is useful when you want to restrict the use of a KMS key to specific AWS resources (e.g., S3 buckets, EBS volumes) not owned by the caller.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Allow attachment of persistent resources",
      "Effect": "Allow",
      "Principal": {
        "AWS": [
          "arn:aws:iam::123456789012:user/admin",
          "arn:aws:iam::123456789012:role/admin"
        ]
      },
      "Action": ["kms:CreateGrant", "kms:ListGrants", "kms:RevokeGrant"],
      "Resource": "*",
      "Condition": {
        "Bool": {
          "kms:GrantIsForAWSResource": "true"
        }
      }
    }
  ]
}
```

## Example 2 - Combined Key Policy

In this example, we combine the key policy and IAM policies into a single policy. This can be useful if you want to manage all permissions in one place.

**Combined Key Policy:**

```json
{
  "Version": "2012-10-17",
  "Id": "key-combined-1",
  "Statement": [
    {
      "Sid": "Enable IAM User Permissions",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::123456789012:root"
      },
      "Action": "kms:*",
      "Resource": "*"
    },
    {
      "Sid": "Allow access for Key Administrators",
      "Effect": "Allow",
      "Principal": {
        "AWS": ["arn:aws:iam::123456789012:role/admin"]
      },
      "Action": [
        "kms:Create*",
        "kms:Describe*",
        "kms:Enable*",
        "kms:List*",
        "kms:Put*",
        "kms:Update*",
        "kms:Revoke*",
        "kms:Disable*",
        "kms:Get*",
        "kms:Delete*",
        "kms:ScheduleKeyDeletion",
        "kms:CancelKeyDeletion"
      ],
      "Resource": "*"
    },
    {
      "Sid": "Allow use of the key",
      "Effect": "Allow",
      "Principal": {
        "AWS": [
          "arn:aws:iam::123456789012:user/admin",
          "arn:aws:iam::123456789012:role/admin"
        ]
      },
      "Action": [
        "kms:Encrypt",
        "kms:Decrypt",
        "kms:ReEncrypt*",
        "kms:GenerateDataKey*",
        "kms:DescribeKey"
      ],
      "Resource": "*"
    },
    {
      "Sid": "Allow attachment of persistent resources",
      "Effect": "Allow",
      "Principal": {
        "AWS": [
          "arn:aws:iam::123456789012:user/admin",
          "arn:aws:iam::123456789012:role/admin"
        ]
      },
      "Action": ["kms:CreateGrant", "kms:ListGrants", "kms:RevokeGrant"],
      "Resource": "*"
    }
  ]
}
```
