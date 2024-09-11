# AWS Control Tower

## Overview

AWS Control Tower simplifies the setup and governance of a secure, compliant multi-account AWS environment based on AWS best practices.

**Key Features**:

1. **Automated Setup**:

   - Automates the creation and configuration of multi-account environments in just a few clicks.
   - Provides ongoing policy management using guardrails.

2. **Policy Management**:

   - **Guardrails**:
     - **Preventive Guardrails**: Implement Service Control Policies (SCPs) to prevent certain actions (e.g., disabling creation of access keys from the root user).
     - **Detective Guardrails**: Use AWS Config to monitor compliance (e.g., ensuring MFA is enabled for the root user).
   - **Monitoring**: Detect policy violations and automate remediation (e.g., tagging untagged resources).

3. **Account Factory**:

   - Automates account provisioning and deployments.
   - Uses AWS Service Catalog to configure new accounts with pre-approved baselines (e.g., default VPC, subnets, regions).

4. **Integration with Active Directory**:

   - Can integrate with Microsoft Active Directory (AD) on-premises through a two-way trust.
   - Accounts created through Control Tower can leverage IAM Identity Center for authentication, using the Active Directory in both the cloud and corporate data center.

5. **Guardrail Levels**:
   - **Mandatory**: Automatically enabled and enforced (e.g., disallow public read access to log archive accounts).
   - **Strongly Recommended**: Based on AWS best practices (e.g., enable EBS volumes attached to EC2 instances).
   - **Elective**: Optional and used by enterprises (e.g., disallow delete actions without MFA in S3 buckets).
