# Restrict AWS access to only approved regions

This SCP control restricts the AWS regions that can be used in your AWS account. This is useful for enforcing compliance and security by limiting the regions that can be used.

Example 1: You only want to allow `us-east-1`, `us-west-2`, and `eu-west-1` regions to be used.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "RequireApprovedRegions",
      "Effect": "Deny",
      "Action": "*",
      "Resource": "*",
      "Condition": {
        "StringNotEquals": {
          "aws:RequestedRegion": ["us-east-1", "us-west-2", "eu-west-1"]
        }
      }
    }
  ]
}
```

Example 2: You want to allow all regions except `cn-north-1` and `cn-northwest-1`.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "RequireApprovedRegions",
      "Effect": "Deny",
      "Action": "*",
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "aws:RequestedRegion": ["cn-north-1", "cn-northwest-1"]
        }
      }
    }
  ]
}
```
