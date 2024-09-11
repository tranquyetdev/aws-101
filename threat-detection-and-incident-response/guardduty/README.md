# AWS GuardDuty

**Document Status**: To create hands-on labs, tutorials, or other learning resources.

## Overview

- **Purpose**: Intelligent threat discovery to protect AWS accounts.
- **Detection Methods**:
  - **Machine Learning**: Identifies anomalies and potential threats.
  - **Anomaly Detection**: Detects unusual behavior or deviations from normal patterns.
  - **Third-Party Data**: Utilizes external threat intelligence.

## Activation and Setup

- **Activation**: Easy one-click setup.
- **Trial**: 30-day free trial available.
- **No Installation**: Requires no additional software installation.

## Data Sources

- **CloudTrail Event Logs**: Monitors API calls, unauthorized deployments, etc.
  - **Management Events**: Examples include creating VPC subnets.
  - **Data Events**: Examples include S3 actions like `get object`, `list objects`. Note, by default, Cloudtrail does not log data events.
- **VPC Flow Logs**: Analyzes internet traffic and unusual IP addresses.
- **DNS Logs**: Detects encoded data in DNS queries indicating compromised instances.
- **Optional Features**:
  - EKS Audit Logs
  - RDS and Aurora Login Events
  - EBS Volumes
  - Lambda Network Activity

## Automated Responses

- **EventBridge Integration**:
  - **Rules**: Automatically respond to findings.
  - **Targets**: Lambda functions, SNS topics, etc.
- **Cryptocurrency Attacks**: GuardDuty has dedicated findings for detecting cryptocurrency mining attacks.

## Multi-Account Strategy with GuardDuty

- **Centralized Management**: You can manage multiple AWS accounts from a single GuardDuty administrator account. This is done using AWS Organizations.
- **Delegated Administration**: A member account can be designated as the GuardDuty administrator, even if it is not the administrator of the AWS Organization itself.
- **Management Tasks**: The administrator account can manage findings, suppression rules, trusted IP lists, and threat lists across all member accounts.

## Findings and Automations

- **EventBridge Integration**: GuardDuty findings are sent to Amazon EventBridge. From EventBridge, you can set up various responses:
  - **Alerts**: Send notifications via SNS, Lambda, Slack, etc.
  - **Automation**: Trigger Lambda functions to take actions like updating network ACLs or creating WAF rules.
- **Sample Findings**: You can generate sample findings in GuardDuty to test your automation setup.

## Finding Types

- **EC2**: Unauthorized access, SSH brute force, cryptocurrency mining.
- **IAM**: CloudTrail logging disabled, root credential usage.
- **Kubernetes**: Access to credentials from malicious IPs.
- **S3**: Public access issues, penetration testing attempts.

## Architecture Examples

1. **EC2 Instance in VPC**:
   - GuardDuty analyzes VPC flow logs and generates findings.
   - Use EventBridge to notify and trigger Lambda functions to block malicious IPs or update network ACLs.
2. **Firewall Subnet**:
   - Use GuardDuty to detect suspicious activity.
   - EventBridge triggers a step function workflow to manage IP blocking via AWS Network Firewall.

## Trusted and Threat IP Lists

- **Trusted IP List**: A list of IPs that you trust; GuardDuty will not generate findings for these IPs.
- **Threat IP List**: A list of known malicious IPs; GuardDuty will generate findings for any activity from these IPs.

## Suppression Rules

- **Purpose**: To filter and archive low-value or false positive findings.
- **Usage**: Define suppression rules for specific finding types or criteria (e.g., findings for certain EC2 instances or ports).
- **Archive**: Suppressed findings are not sent to Security Hub, S3, Detective, or EventBridge but can still be viewed in the GuardDuty archive.

## Troubleshooting

- **DNS Findings**: GuardDuty only processes DNS logs if using the default VPC DNS resolver. Custom DNS resolvers will not generate DNS-based findings.
- **GuardDuty Suspension**: If GuardDuty is disabled or suspended, no findings will be generated.

## Best Practices

- **Enable GuardDuty**: Even in regions you don’t actively use, to detect potential threats in all regions.
