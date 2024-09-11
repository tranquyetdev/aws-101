# EC2 Linux Lost Key Pair

**Document Status**: To create hands-on labs, tutorials, or other learning resources.

1. **EC2 User Data**:

   - **Create a new key pair** and copy the public key.
   - **Stop the instance** and update the EC2 User Data with the new public key.
   - **Start the instance**; it will add the new key to the authorized keys file.
   - **Delete the EC2 User Data** after the one-time setup to avoid adding keys on every boot.

2. **Systems Manager**:

   - **Ensure the SSM agent is installed** and the correct IAM Role is attached.
   - **Run the AWSSupport-ResetAccess automation** from the Systems Manager Console.
   - **Retrieve the new private key** from Parameter Store and use it to SSH into the instance.
   - **Delete the private key** from Parameter Store after use.

3. **EC2 Instance Connect Service**:

   - **Use the EC2 Instance Connect Service** (available on Amazon Linux 2 or Ubuntu 16 or later).
   - **Connect temporarily to the instance** and modify the SSH authorized keys file to add the new public key.

4. **EC2 Serial Console**:

   - **Enable EC2 Serial Console** at the account level (only for Nitro-based instances).
   - **Connect via the Serial Console** and manually add the new public key to the SSH authorized keys file.
   - **Fix any network connectivity issues** if applicable.

5. **EBS Volume Swap**:
   - **Create a new key pair** and stop the original instance.
   - **Detach the EBS root volume** and attach it to a new EC2 instance.
   - **SSH into the new instance** and modify the authorized keys file on the attached volume.
   - **Detach the volume** from the new instance, reattach it to the original instance, and restart it.

These methods provide various ways to regain access to an EC2 instance depending on the tools and configurations available.
