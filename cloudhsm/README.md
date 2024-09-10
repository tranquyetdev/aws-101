# CloudHSM

CloudHSM is a cloud-based hardware security module (HSM) that enables you to easily generate and use your own encryption keys on the AWS Cloud. With CloudHSM, you can manage your own encryption keys using FIPS 140-2 Level 3 validated HSMs.

## Key Takeaways

1. **CloudHSM vs KMS**:

   - **KMS**: AWS manages both the encryption software and keys.
   - **CloudHSM**: AWS only provides dedicated hardware (Hardware Security Module), and **you** are entirely responsible for managing and securing the encryption keys.

2. **Responsibility for Keys**:

   - With **CloudHSM**, the user is responsible for the **security, management, and backup** of the encryption keys.
   - AWS has no access to the keys and cannot recover them if lost.

3. **Tamper Resistance and Compliance**:

   - CloudHSM devices are **tamper-resistant** and compliant with security standards (e.g., **FIPS 140-2 Level 3**).
   - If the HSM is tampered with, the device will log and signal it.

4. **Symmetric and Asymmetric Encryption**:

   - CloudHSM supports both **symmetric** and **asymmetric** encryption, making it suitable for **TLS/SSL offloading** and other cryptographic tasks.

5. **Client Software Requirement**:

   - To use CloudHSM, you must install **CloudHSM Client Software** to interact with the device. Unlike KMS, it is not managed via API calls.

6. **Integration with Other AWS Services**:

   - CloudHSM integrates with services like **Amazon Redshift** for database encryption and **S3** for customer-managed encryption keys (SSE-C).

7. **High Availability Setup**:
   - CloudHSM requires manual setup for **high availability** by deploying multiple HSM devices across **multiple Availability Zones (AZs)**. This enhances the **availability and durability** of the data.
8. **Single-Tenant vs Multi-Tenant**:

   - **CloudHSM** is **single-tenant**, meaning the device is dedicated to one customer.
   - **KMS** is **multi-tenant**, shared across AWS customers.

9. **Key Management**:

   - CloudHSM only supports **Customer Managed CMK** (Customer Master Key).
   - In KMS, there are various types of keys like **AWS Owned Keys**, **AWS Managed Keys**, and **Customer Managed Keys**.

10. **Cryptographic Features**:

    - CloudHSM provides cryptographic acceleration for **SSL/TLS** and **Oracle TDE**.
    - Supports both **symmetric, asymmetric encryption, digital signing**, and **hashing**.

11. **Access and Authorization**:

    - **KMS**: Access is managed via **IAM** (Identity and Access Management).
    - **CloudHSM**: Users and permissions are managed within the CloudHSM device itself.

12. **Audit and Logging**:

    - Both services support **CloudTrail** and **CloudWatch** for auditing.
    - CloudHSM also supports **multifactor authentication (MFA)**.

13. **Cost**:
    - **KMS** has a **free tier**, but **CloudHSM** does not offer any free tier.

### **Use Cases for CloudHSM**:

- Full control over cryptographic keys with **no third-party access**.
- Organizations needing **compliance** with strict security standards (FIPS 140-2 Level 3).
- Secure encryption for **SSL/TLS offloading**, **Oracle TDE**, and **custom key management** across multiple environments.
- High-security environments where tamper-resistant hardware is required.

In summary, CloudHSM provides dedicated encryption hardware and requires the user to manage all aspects of key security and availability, making it ideal for high-security use cases with strict compliance needs.
