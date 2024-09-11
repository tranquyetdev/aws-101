# Service Control Policies

Service Control Policies (SCPs) are a type of policy that you can use to manage permissions in your organization. SCPs offer central control over the maximum available permissions for all accounts in your organization. SCPs are similar to IAM permission policies, but they are more powerful. An SCP specifies the maximum permissions for an organization, organizational unit (OU), or account. SCPs are not tied to a specific service, API, or action. Instead, they are a whitelist of actions that you allow, which means that you can use them to ensure that your accounts remain within the permissions guardrails that you set.

## Why DENY is Preferred for SCPs:

By default, FullAWSAccess is granted to all accounts in an organization. This means that all actions are allowed unless explicitly denied. To restrict permissions, you can use SCPs to deny specific actions across all accounts in an organization or organizational unit (OU). When using SCPs, it is recommended to use **DENY** statements to restrict actions, rather than **ALLOW** statements. This approach helps ensure that you are following the principle of least privilege and that you are not inadvertently granting more permissions than intended.

1. **SCPs Work as Guardrails**:

   - SCPs are designed to act as **guardrails** that restrict or block certain actions across all accounts within an organization or organizational unit (OU).
   - They are not used to **grant permissions** directly but to **restrict** what accounts can do, in addition to the permissions defined in IAM policies at the account level.

2. **Principle of Least Privilege**:

   - Using DENY statements ensures that you are following the **principle of least privilege** by blocking only specific actions that could pose security risks.
   - Allowing actions via SCPs could inadvertently grant more permissions than intended, especially when combining them with the permissions of individual account-level IAM policies.

3. **Preventing Unintended Permissions**:

   - If you use ALLOW in SCPs, you risk granting permissions that may not have been explicitly intended in individual accounts, as SCPs override IAM policies. This can lead to **unintended escalation** of permissions.
   - AWS Organizations is designed so that SCPs enforce **restrictions** across accounts, while individual accounts define more granular permissions via IAM.

4. **IAM Policies are for Allowing**:
   - IAM policies should handle the **ALLOW** logic within each account, determining what actions specific users, roles, or services are permitted to perform.
   - SCPs should only **DENY** actions that need to be globally restricted across the organization, adding another layer of security on top of the IAM policies.

### Example: Using SCPs with DENY

For instance, if you want to ensure that no one in the organization can disable logging, you would use a **Deny** action in the SCP to block `s3:PutBucketLogging` or `cloudtrail:StopLogging`. This doesn't interfere with the specific permissions granted by IAM policies in each account.

### When to Use DENY in SCPs:

- Prevent risky actions, such as disabling security configurations (e.g., stopping CloudTrail or modifying IAM roles).
- Block access to regions where your organization does not operate.
- Enforce security controls (e.g., requiring encryption, IMDSv2, or restricting public access to S3).
- Ensure compliance with internal or regulatory policies.

### Example: Denying Actions in SCPs

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": ["ec2:RunInstances"],
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

### Conclusion:

- **SCPs should primarily define DENY actions** to enforce restrictions across AWS accounts.
- Use **IAM policies** at the account level to define granular permissions and **allow** necessary actions for users and roles.
- Keeping SCPs focused on **denying risky or unwanted actions** helps ensure security and compliance across the organization without inadvertently granting more access than intended.

This approach keeps control clear and secure, leveraging SCPs for organization-wide restrictions and IAM policies for specific permissions.

## Recipes

- [Restric EC2 Instance Types](recipes/restrict-ec2-instance-types.md)
- [Restrict EC2 IMDS v2](recipes/restrict-ec2-imds-v2.md)
- [Restrict AWS Regions](recipes/restrict-aws-regions.md)
