# Restrict EC2 Instance Types

This SCP control restricts the EC2 instance types that can be launched in your AWS account. This is useful for enforcing compliance and cost control by limiting the instance types that can be used.

Example: You only want to allow `t2.micro`, `t3.micro`, and `t3.small` instances to be launched.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "RequireInstanceTypes",
      "Effect": "Deny",
      "Action": "ec2:RunInstances",
      "Resource": "*",
      "Condition": {
        "StringNotEquals": {
          "ec2:InstanceType": ["t2.micro", "t3.micro", "t3.small"]
        }
      }
    }
  ]
}
```
