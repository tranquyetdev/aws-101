# EC2 Windows Lost Password Recovery

**Document Status**: To create hands-on labs, tutorials, or other learning resources.

1. **EC2Launch v2 (Windows Server 2016 and Later)**:

   - **Detach the EBS root volume** from the instance.
   - **Attach it to a temporary Windows EC2 instance**.
   - **Delete the "run-once" file** on the secondary volume.
   - **Reattach and restart** the original instance.
   - **Enter a new password** when prompted.

2. **EC2Config Service (Older AMIs Before Windows Server 2016)**:

   - **Create a new EC2 instance**.
   - **Modify the EC2Config XML file** to set `EC2 set password` to `enabled`.
   - **Restart the original instance** to set a new password.

3. **EC2Launch (Older Windows Server Versions, Not Updated to v2)**:

   - **Download and install EC2Rescue Tool for Windows Server**.
   - **Use the "Reset Administrator Password" option** in EC2Rescue Tool.
   - **Reattach the volume** to the original instance and restart it.

4. **Systems Manager**:
   - **Ensure the instance has SSM Agent installed**.
   - **Use one of the following methods**:
     - **AWSSupport-RunEC2RescueForWindows Tool**: Installs EC2Rescue Tool for password reset.
     - **Automation Document "ResetAccess"**: Automated password reset.
     - **Run Command Document "RunPowerShellScript"**: Set password via PowerShell script.

These methods provide various approaches for password recovery, depending on the Windows AMI and tools available.
