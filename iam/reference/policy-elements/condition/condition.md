# IAM Condition Operators

IAM policies support a number of condition operators that you can use to specify when a policy should be applied.
Ref: [IAM JSON policy elements: Condition operators](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html)

The following are the condition operators that you can use in an IAM policy:

## String Operators

- `StringEquals`: Compares two strings to determine if they are equal.
- `StringNotEquals`: Compares two strings to determine if they are not equal.
- `StringEqualsIgnoreCase`: Compares two strings to determine if they are equal, ignoring the case of the strings.
- `StringNotEqualsIgnoreCase`: Compares two strings to determine if they are not equal, ignoring the case of the strings.

Example: This policy allows access to an S3 bucket only if the principal tag `department` is set to `engineering`.

```json
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Action": "s3:GetObject",
    "Resource": "arn:aws:s3:::example-bucket/*",
    "Condition": {
      "StringEquals": {
        "aws:PrincipalTag/department": "engineering"
      }
    }
  }
}
```

- `StringLike`: Compares a string to a pattern to determine if the string matches the pattern. The pattern can contain the wildcard characters `*` and `?`.
- `StringNotLike`: Compares a string to a pattern to determine if the string does not match the pattern. The pattern can contain the wildcard characters `*` and `?`.

Example 1: This policy allow access to an S3 bucket only if the principal tag `department` is set to `engineering` or `sales`.

```json
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Action": "s3:GetObject",
    "Resource": "arn:aws:s3:::example-bucket/*",
    "Condition": {
      "StringLike": {
        "aws:PrincipalTag/department": ["engineering", "sales"]
      }
    }
  }
}
```

Example 2: This policy allows access to S3 bucket based on s3:prefix condition. The policy allows access to the `home` folder of the user.

```json
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Action": "s3:GetObject",
    "Resource": "arn:aws:s3:::example-bucket/*",
    "Condition": {
      "StringLike": {
        "s3:prefix": "home/${aws:username}/*"
      }
    }
  }
}
```

## Numeric Operators

- `NumericEquals`: Compares two numbers to determine if they are equal.
- `NumericNotEquals`: Compares two numbers to determine if they are not equal.
- `NumericLessThan`: Compares two numbers to determine if the first number is less than the second number.
- `NumericLessThanEquals`: Compares two numbers to determine if the first number is less than or equal to the second number.
- `NumericGreaterThan`: Compares two numbers to determine if the first number is greater than the second number.
- `NumericGreaterThanEquals`: Compares two numbers to determine if the first number is greater than or equal to the second number.

Example: This policy allows access to an S3 bucket only if the request is made with a specific content length.

```json
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Action": "s3:PutObject",
    "Resource": "arn:aws:s3:::example-bucket/*",
    "Condition": {
      "NumericEquals": {
        "s3:content-length-range": ["100", "200"]
      }
    }
  }
}
```

## Date Operators

- `DateEquals`: Compares two dates to determine if they are equal.
- `DateNotEquals`: Compares two dates to determine if they are not equal.
- `DateLessThan`: Compares two dates to determine if the first date is before the second date.
- `DateLessThanEquals`: Compares two dates to determine if the first date is before or equal to the second date.
- `DateGreaterThan`: Compares two dates to determine if the first date is after the second date.
- `DateGreaterThanEquals`: Compares two dates to determine if the first date is after or equal to the second date.

Example: This policy allows access to an S3 bucket only if the request is made before a specific date.

```json
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Action": "s3:GetObject",
    "Resource": "arn:aws:s3:::example-bucket/*",
    "Condition": {
      "DateLessThan": {
        "aws:CurrentTime": "2022-01-01T00:00:00Z"
      }
    }
  }
}
```

## Boolean Operators

- `Bool`: Compares a boolean value to determine if it is true.
- `BoolIfExists`: Compares a boolean value to determine if it exists.

Example: This policy allows to delete an S3 bucket object only if the request is made with MFA.

```json
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Action": "s3:DeleteObject",
    "Resource": "arn:aws:s3:::example-bucket/*",
    "Condition": {
      "Bool": {
        "aws:MultiFactorAuthPresent": "true"
      }
    }
  }
}
```

Example: This policy checks for aws:SecureTransport condition to deny access to an S3 bucket if the request is not made over HTTPS.

```json
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Deny",
    "Action": "s3:*",
    "Resource": [
      "arn:aws:s3:::example-bucket",
      "arn:aws:s3:::example-bucket/*"
    ],
    "Condition": {
      "Bool": {
        "aws:SecureTransport": "false"
      }
    }
  }
}
```

## Binary Operators

- `BinaryEquals`: Compares two binary values to determine if they are equal.
- `BinaryNotEquals`: Compares two binary values to determine if they are not equal.

Example: This policy allows access to an S3 bucket only if the request is made with a specific header value.

```json
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Action": "s3:GetObject",
    "Resource": "arn:aws:s3:::example-bucket/*",
    "Condition": {
      "BinaryEquals": {
        "aws:headerName": "headerValue"
      }
    }
  }
}
```

## ARN Operators

- `ArnEquals`: Compares two ARNs to determine if they are equal.
- `ArnNotEquals`: Compares two ARNs to determine if they are not equal.
- `ArnLike`: Compares an ARN to a pattern to determine if the ARN matches the pattern. The pattern can contain the wildcard characters `*` and `?`.
- `ArnNotLike`: Compares an ARN to a pattern to determine if the ARN does not match the pattern. The pattern can contain the wildcard characters `*` and `?`.

Example: This policy allows access to an S3 bucket only if the request is made from a specific ARN.

```json
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Action": "s3:GetObject",
    "Resource": "arn:aws:s3:::example-bucket/*",
    "Condition": {
      "ArnEquals": {
        "aws:PrincipalArn": "arn:aws:iam::123456789012:user/user-name"
      }
    }
  }
}
```

## IP Address Operators

- `IpAddress`: Compares an IP address to a range of IP addresses to determine if the IP address is in the range.
- `NotIpAddress`: Compares an IP address to a range of IP addresses to determine if the IP address is not in the range.

Example: This policy denies access to an S3 bucket if the request is made from an IP address outside the specified range.

```json
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Deny",
    "Action": "s3:*",
    "Resource": [
      "arn:aws:s3:::example-bucket",
      "arn:aws:s3:::example-bucket/*"
    ],
    "Condition": {
      "NotIpAddress": {
        "aws:SourceIp": "123.0.112.0/24"
      }
    }
  }
}
```

## Null Operators

- `Null`: Compares a value to determine if it is null.
- `NullIfExists`: Compares a value to determine if it exists.

Example: You can use this condition operator to determine whether a user is using their own credentials for the operation or temporary credentials. If the user is using temporary credentials, then the key aws:TokenIssueTime exists and has a value. The following example shows a condition that states that the user must not be using temporary credentials (the key must not exist) for the user to use the Amazon EC2 API.

```json
{
  "Version": "2012-10-17",
  "Statement": {
    "Action": "ec2:*",
    "Effect": "Allow",
    "Resource": "*",
    "Condition": { "Null": { "aws:TokenIssueTime": "true" } }
  }
}
```

## CalledVia Operators

The `CalledVia` condition key is used to control access to AWS services that are called by other AWS services on behalf of the IAM User or Role.
**Supported services:**

- athena.amazonaws.com
- cloudformation.amazonaws.com
- dynamodb.amazonaws.com
- kms.amazonaws.com

Example: This policy allows access to an S3 bucket only if the request is made by Athena service.

```json
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Action": "s3:GetObject",
    "Resource": "arn:aws:s3:::example-bucket/*",
    "Condition": {
      "StringEquals": {
        "aws:CalledVia": "athena.amazonaws.com"
      }
    }
  }
}
```

## IP and VPC Conditions

- aws:SourceIp
  - Public requester IP (e.g., public EC2 IP if coming from EC2)
  - Not present in requests through VPC Endpoints
- aws:VpcSourceIp – requester IP through VPC Endpoints (private IP)
- aws:SourceVpce – restrict access to a specific VPC Endpoint
- aws:SourceVpc
  - Restrict to a specific VPC ID
  - Request must be made through a VPC Endpoint
- Common to use these conditions with S3 Bucket Policies

## Global condition Keys

When a principal makes a request to AWS, AWS gathers the request information into a request context. You can use the Condition element of a JSON policy to compare keys in the request context with key values that you specify in your policy. Request information is provided by different sources, including the principal making the request, the resource the request is made against, and the metadata about the request itself.

https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html

## IAM and AWS STS condition context keys

https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_iam-condition-keys.html#available-keys-for-iam
