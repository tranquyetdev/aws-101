# Compromised AWS Resources

**Document Status**: To create hands-on labs, tutorials, or other learning resources.

## Compromised EC2 Instance

1. **Capture Metadata**: Gather instance metadata and enable termination protection to prevent accidental deletion.
2. **Isolate the Instance**: Change the security group to one with no outbound traffic to contain the instance.
3. **Detach from Auto-Scaling**: Suspend auto-scaling processes and detach the instance to prevent automatic replacements.
4. **De-register from ELB**: Remove the instance from any load balancer to stop it from receiving network traffic.
5. **Snapshot EBS Volumes**: Create snapshots of any attached EBS volumes for analysis.
6. **Forensic Analysis**:
   - **Offline Investigation**: Shut down the instance, restore and attach EBS snapshots to a forensic instance.
   - **Online Investigation**: Capture memory and network traffic while the instance is still running to detect data leaks.
7. **Automation**: Use AWS Lambda and SSM run commands for automating memory capture and other processes.
8. **Tagging**: Label the compromised instance with an investigation ticket number.

## Compromised S3 Bucket

1. **Identify Compromise**: Use GuardDuty to detect compromised S3 buckets.
2. **Analyze Activity**: Use CloudTrail or Amazon Detective to trace the source of malicious activity.
3. **Reinforce Security**:
   - Block public access.
   - Review and update bucket policies and IAM policies.
   - Use VPC endpoints, S3 pre-signed URLs, and S3 access points.

## Compromised ECS Cluster

1. **Identify Compromise**: Use GuardDuty to detect the compromised cluster and analyze the source of the issue (e.g., container images, tasks).
2. **Isolate and Escalate**:
   - Deny ingress and egress traffic using a new security group.
   - Evaluate and address malware or malicious activity.
3. **Forensics**: Use GuardDuty’s snapshot feature to retain data and analyze it for malicious activity.

## Compromised Standalone Container

1. **Isolate Container**: Apply a new security group to limit traffic.
2. **Suspend or Stop**: Pause or stop the container to prevent further damage.
3. **Snapshot and Analyze**: Use EBS snapshots for forensic analysis.

## Compromised RDS Database

1. **Detect and Restrict**: Use GuardDuty to identify and restrict network access and database access.
2. **Rotate Credentials**: Change passwords for compromised users to prevent further unauthorized access.
3. **Review Audit Logs**: Check database audit logs for leaked data.
4. **Enhance Security**:
   - Use Secrets Manager for password rotation.
   - Implement IAM database authentication for access control.
