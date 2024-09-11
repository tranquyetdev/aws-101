# Compromised AWS Credentials

**Document Status**: To create hands-on labs, tutorials, or other learning resources.

## Compromised IAM User Credentials

1. **Rotate Credentials**: Immediately rotate the compromised credentials to prevent further use.
2. **Invalidate Old Credentials**: Attach an explicit deny policy with an STS date condition to the affected IAM user. This policy ensures that any credentials issued before a specified date are invalidated.
3. **Check CloudTrail Logs**: Review logs for any unauthorized activities.
4. **Review Resources**: Assess any changes or deletions to resources that might have occurred due to unauthorized actions.
5. **Verify Account Information**: Confirm that no other account information has been altered.

## Compromised IAM Role

1. **Identify Compromised Role**: Determine which IAM role has been compromised.
2. **Attach Deny Policy**: Apply an explicit deny policy similar to the IAM user case to prevent actions from tokens issued before today’s date.
3. **Revoke Access**: If using Active Directory or SSO, revoke access from the linked identity.
4. **Check CloudTrail Logs**: Look for unauthorized activities related to the compromised role.
5. **Review Resources and Verify Information**: Similar to the IAM user case, ensure resources are intact and verify account details.

## Compromised AWS Account

1. **Rotate All Keys**: Delete and rotate all access keys for AWS, including those for IAM users and roles.
2. **Delete Unauthorized IAM Users**: Remove any unauthorized IAM users and rotate their passwords.
3. **Rotate EC2 Key Pairs**: Update and delete all EC2 key pairs to prevent unauthorized SSH access.
4. **Check CloudTrail Logs**: Examine logs for any signs of unauthorized activities.
5. **Review Resources and Account Information**: Assess all resources for any unwanted changes or deletions and ensure that account information is correct.
