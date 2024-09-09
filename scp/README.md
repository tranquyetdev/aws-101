# Service Control Policies

Service Control Policies (SCPs) are a type of policy that you can use to manage permissions in your organization. SCPs offer central control over the maximum available permissions for all accounts in your organization. SCPs are similar to IAM permission policies, but they are more powerful. An SCP specifies the maximum permissions for an organization, organizational unit (OU), or account. SCPs are not tied to a specific service, API, or action. Instead, they are a whitelist of actions that you allow, which means that you can use them to ensure that your accounts remain within the permissions guardrails that you set.

## Recipes

- [Restric EC2 Instance Types](recipes/restrict-ec2-instance-types.md)
- [Restrict EC2 IMDS v2](recipes/restrict-ec2-imds-v2.md)
- [Restrict AWS Regions](recipes/restrict-aws-regions.md)
