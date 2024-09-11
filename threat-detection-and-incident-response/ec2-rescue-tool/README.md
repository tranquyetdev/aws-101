# EC2Rescue Tool

**Document Status**: To create hands-on labs, tutorials, or other learning resources.

## General Overview

- **EC2Rescue**: A tool for diagnosing and troubleshooting common issues on EC2 instances.
- **Available for Both Linux and Windows**.
- **Can perform tasks like gathering logs**, diagnosing kernel issues, and fixing common problems.
- **Can output data to AWS Support** or upload results to an S3 bucket.

## EC2Rescue for Linux

- **Supported Distributions**: Amazon Linux 2, Ubuntu, Red Hat, and others.
- **Installation**: Can be done manually or via SSM automation.
- **Capabilities**:
  - **Collect System Utilization Reports**: vmstate, iostat, mpstats.
  - **Gather Logs and Details**: syslog, dmesg, application error logs, SSM logs.
  - **Detect and Remediate Problems**: Asymmetric routing, duplicate root device labels, OpenSSH file permissions, non-problematic kernel problems.
  - **Custom Modules**: Users can create their own modules for specific needs.

## EC2Rescue for Windows

- **Supported Versions**: Windows Server 2008 and later.
- **Installation**: Run commands via SSM, such as `CollectLogs`, `FixAll`, or `ResetAccess`.
- **Capabilities**:
  - **Diagnose and Troubleshoot**: Instance connectivity issues, OS boot problems, and common OS issues.
  - **Log Collection**: Gathers OS logs, configuration files, and performs advanced log analysis.
  - **Common Issues**: Connectivity problems (firewall, RDP), boot issues (blue screen, boot loops), disk signature collisions, corrupted registry.
  - **Restoration**: Can perform system restores.

The EC2Rescue tool provides a comprehensive set of features for diagnosing and resolving issues on EC2 instances, with specific tools and modules tailored to both Linux and Windows environments.
