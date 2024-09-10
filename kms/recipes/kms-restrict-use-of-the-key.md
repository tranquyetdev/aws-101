# KMS Restrict Use of the Key

## References

- [AWS Global Condition Keys](https://docs.aws.amazon.com/kms/latest/developerguide/conditions-aws.html)
- [AWS KMS condition keys](https://docs.aws.amazon.com/kms/latest/developerguide/conditions-kms.html)

## kms:ViaService

Use the `kms:ViaService` condition key to limit the use of a KMS key to requests from specific AWS services. This condition key is useful when you want to restrict the use of a KMS key to a specific AWS service, such as Amazon EC2 or AWS Lambda.

When you use the `kms:ViaService` condition key, you specify the AWS service principal that is making the request. The AWS service principal is the service that is making the request on behalf of the user. For example, when an Amazon EC2 instance makes a request to use a KMS key, the request is made on behalf of the user who launched the instance.

The following example shows a key policy that allows the use of a KMS key only for requests from Amazon EC2 and AWS Lambda:

```json
{
  "Version": "2012-10-17",
  "Id": "key-default-1",
  "Statement": [
    {
      "Sid": "Allow use of the key",
      "Effect": "Allow",
      "Principal": {
        "AWS": [
          "arn:aws:iam::123456789012:user/user-name",
          "arn:aws:iam::123456789012:role/role-name"
        ]
      },
      "Action": [
        "kms:Encrypt",
        "kms:Decrypt",
        "kms:ReEncrypt*",
        "kms:GenerateDataKey*",
        "kms:DescribeKey",
        "kms:CreateGrant",
        "kms:ListGrants",
        "kms:RevokeGrant"
      ],
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "kms:ViaService": ["ec2.amazonaws.com", "lambda.amazonaws.com"]
        }
      }
    }
  ]
}
```

## kms:CallerAccount

Use the `kms:CallerAccount` condition key to limit the use of a KMS key to requests from specific AWS accounts. This condition key is useful when you want to restrict the use of a KMS key to specific AWS accounts.

When you use the `kms:CallerAccount` condition key, you specify the AWS account ID of the account that is making the request. The following example shows a key policy that allows the use of a KMS key only for requests from a specific AWS account:

```json
{
  "Version": "2012-10-17",
  "Id": "key-default-1",
  "Statement": [
    {
      "Sid": "Allow use of the key",
      "Effect": "Allow",
      "Principal": {
        "AWS": [
          "arn:aws:iam::123456789012:user/user-name",
          "arn:aws:iam::123456789012:role/role-name"
        ]
      },
      "Action": [
        "kms:Encrypt",
        "kms:Decrypt",
        "kms:ReEncrypt*",
        "kms:GenerateDataKey*",
        "kms:DescribeKey",
        "kms:CreateGrant",
        "kms:ListGrants",
        "kms:RevokeGrant"
      ],
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "kms:CallerAccount": "123456789012",
          "kms:ViaService": ["ec2.amazonaws.com", "lambda.amazonaws.com"]
        }
      }
    }
  ]
}
```
