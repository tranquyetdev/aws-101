# Force launching EC2 instances with IMDSv2 enabled using SCP

Here's an example of a **Service Control Policy (SCP)** that enforces launching EC2 instances with **Instance Metadata Service Version 2 (IMDSv2)** enabled. This SCP denies the ability to launch or run EC2 instances without IMDSv2, ensuring that only instances with IMDSv2 are allowed.

## Force launching EC2 instances with IMDSv2 enabled using SCP

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": "ec2:RunInstances",
      "Resource": "*",
      "Condition": {
        "StringNotEquals": {
          "ec2:MetadataHttpTokens": "required"
        }
      }
    },
    {
      "Effect": "Deny",
      "Action": [
        "ec2:ModifyInstanceMetadataOptions",
        "ec2:ModifyInstanceAttribute"
      ],
      "Resource": "*",
      "Condition": {
        "StringNotEquals": {
          "ec2:MetadataHttpTokens": "required"
        }
      }
    }
  ]
}
```

This policy does the following:

- **Deny `ec2:RunInstances`**: Prevents launching new EC2 instances unless **IMDSv2** (`MetadataHttpTokens: required`) is enforced.
- **Deny `ec2:ModifyInstanceMetadataOptions` and `ec2:ModifyInstanceAttribute`**: Prevents modification of instance metadata settings for existing EC2 instances unless IMDSv2 is required.

This ensures that all EC2 instances in the organization use **IMDSv2** for metadata access.

### Applying the SCP:

1. **Attach to an AWS Organization Root, OU, or Account**: Apply this SCP at the desired level in your AWS Organizations hierarchy (root, organizational unit, or specific accounts) to enforce this policy across all applicable accounts.
2. **Effect**: Once applied, this SCP ensures that EC2 instances across your organization can only be launched with IMDSv2 enabled.
