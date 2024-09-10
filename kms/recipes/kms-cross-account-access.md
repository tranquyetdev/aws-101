# KMS Cross Account Access

In some cases, we need to share the KMS key across multiple AWS accounts. This can be done using the KMS key policy and IAM policies. The KMS key policy defines who can use the KMS key and what actions they can perform. The IAM policies define who can use the KMS key and what actions they can perform.

Common use cases for cross-account access to a KMS key include:

- Sharing encrypted data across multiple AWS accounts. For example, sharing encrypted EBS snapshots across multiple AWS accounts.
  - The KMS key policy allows the AWS accounts to use the KMS key.
  - The IAM policies allow the IAM users/roles in the AWS accounts to use the KMS key.
  - Then the IAM users/roles can use the KMS key to encrypt/decrypt the data.
- Centralized key management. For example, managing the KMS key in a central AWS account and allowing other AWS accounts to use the KMS key.
  - The KMS key policy allows the AWS accounts to use the KMS key.
  - The IAM policies allow the IAM users/roles in the AWS accounts to use the KMS key.
  - Then the IAM users/roles can use the KMS key to encrypt/decrypt the data.

In this recipe, we will cover how to allow IAM users/roles from another AWS account to use a KMS key.

## Allow IAM users/roles from another AWS account to use a KMS key

**Context:**

- Account A (123456789012) - KMS key owner
- Account B (210987654321) - IAM role to use the KMS key

**Steps:**

- Create a KMS key in Account A
- Attach a key policy to the KMS key in Account A. The key policy should allow the Account B to use the KMS key.
  - Without allowing cross-account grants

**Note:** We can either allow the Account B to use the KMS key and the Account B can decide which IAM roles can use the KMS key or we can restrict specific IAM roles from Account B to use the KMS key.

```json
{
  "Version": "2012-10-17",
  "Id": "key-default-1",
  "Statement": [
    {
      "Sid": "Allow use of the key",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::210987654321:root"
      },
      "Action": [
        "kms:Encrypt",
        "kms:Decrypt",
        "kms:ReEncrypt*",
        "kms:DescribeKey",
        "kms:GenerateDataKey*"
      ],
      "Resource": "*"
    }
  ]
}
```

- Create an IAM role in Account B
- Attach an IAM policy to the IAM role in Account B to allow the role to use the KMS key in Account A. The IAM policy should allow the role to use the KMS key for encryption, decryption, re-encryption, and key description.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "kms:Encrypt",
        "kms:Decrypt",
        "kms:ReEncrypt*",
        "kms:DescribeKey",
        "kms:GenerateDataKey*"
      ],
      "Resource": "arn:aws:kms:us-east-1:123456789012:key/1234abcd-12ab-34cd-56ef-1234567890ab"
    }
  ]
}
```

- Test the IAM role in Account B to use the KMS key in Account A for encryption, decryption, re-encryption, and key description. Use CloudShell in the AWS Management Console in Account B to run the following commands:

```bash
# Assume the IAM role in Account B
aws sts assume-role --role-arn arn:aws:iam::210987654321:role/role-name --role-session-name test-session | jq -r '.Credentials | "export AWS_ACCESS_KEY_ID=\(.AccessKeyId)\nexport AWS_SECRET_ACCESS_KEY=\(.SecretAccessKey)\nexport AWS_SESSION_TOKEN=\(.SessionToken)"' | bash

# Describe the KMS key in Account A
aws kms describe-key --key-id arn:aws:kms:us-east-1:123456789012:key/1234abcd-12ab-34cd-56ef-1234567890ab

# Encrypt a plaintext file using the KMS key in Account A
echo "Hello, World!" > plaintext.txt
aws kms encrypt --key-id arn:aws:kms:us-east-1:123456789012:key/1234abcd-12ab-34cd-56ef-1234567890ab --plaintext fileb://plaintext.txt --output text --query CiphertextBlob | base64 --decode > ciphertext.txt

# Decrypt the ciphertext using the KMS key in Account A
aws kms decrypt --ciphertext-blob fileb://ciphertext.txt --output text --query Plaintext | base64 --decode

# Re-encrypt the ciphertext using the KMS key in Account A
aws kms re-encrypt --ciphertext-blob fileb://ciphertext.txt --destination-key-id arn:aws:kms:us-east-1:123456789012:key/1234abcd-12ab-34cd-56ef-1234567890ab --output text --query CiphertextBlob | base64 --decode

# Generate a data key using the KMS key in Account A
aws kms generate-data-key --key-id arn:aws:kms:us-east-1:123456789012:key/1234abcd-12ab-34cd-56ef-1234567890ab --key-spec AES_256 --output text --query CiphertextBlob | base64 --decode
```

## Allow IAM users/roles from another AWS account to create grants for a KMS key

**Context:**

- Account A (123456789012) - KMS key owner
- Account B (210987654321) - IAM role to create grants for the KMS key
  - The IAM role in Account B should be allowed to create grants for the KMS key in Account A.
  - Example: An EC2 instance takes this role to create grants for the KMS key in Account A to encrypt/decrypt EBS volumes.

**Steps:**

- Create a KMS key in Account A
- Attach a key policy to the KMS key in Account A. The key policy should allow the Account B to create grants for the KMS key.

```json
{
  "Version": "2012-10-17",
  "Id": "key-default-1",
  "Statement": [
    {
      "Sid": "Allow use of the key",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::210987654321:root"
      },
      "Action": [
        "kms:Encrypt",
        "kms:Decrypt",
        "kms:ReEncrypt*",
        "kms:DescribeKey",
        "kms:GenerateDataKey*",
        "kms:CreateGrant",
        "kms:ListGrants",
        "kms:RevokeGrant"
      ],
      "Resource": "*"
    }
  ]
}
```

- Create an IAM role in Account B
- Attach an IAM policy to the IAM role in Account B to allow the role to create grants for the KMS key in Account A. The IAM policy should allow the role to create grants for the KMS key in Account A.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "kms:Encrypt",
        "kms:Decrypt",
        "kms:ReEncrypt*",
        "kms:DescribeKey",
        "kms:GenerateDataKey*",
        "kms:CreateGrant",
        "kms:ListGrants",
        "kms:RevokeGrant"
      ],
      "Resource": "arn:aws:kms:us-east-1:123456789012:key/1234abcd-12ab-34cd-56ef-1234567890ab"
    }
  ]
}
```

- Assign the IAM role in Account B to an EC2 instance.
- Test the IAM role in Account B to create grants for the KMS key in Account A. Use CloudShell in the AWS Management Console in Account B to run the following commands:

```bash
# Assume the IAM role in Account B
aws sts assume-role --role-arn arn:aws:iam::210987654321:role/role-name --role-session-name test-session | jq -r '.Credentials | "export AWS_ACCESS_KEY_ID=\(.AccessKeyId)\nexport AWS_SECRET_ACCESS_KEY=\(.SecretAccessKey)\nexport AWS_SESSION_TOKEN=\(.SessionToken)"' | bash

# Create a grant for the KMS key in Account A
aws kms create-grant --key-id arn:aws:kms:us-east-1:123456789012:key/1234abcd-12ab-34cd-56ef-1234567890ab --grantee-principal arn:aws:iam::210987654321:role/role-name --operations Encrypt Decrypt ReEncrypt* GenerateDataKey* DescribeKey

# List the grants for the KMS key in Account A
aws kms list-grants --key-id arn:aws:kms:us-east-1:123456789012:key/1234abcd-12ab-34cd-56ef-1234567890ab

# Use the grant to encrypt a plaintext file using the KMS key in Account A
echo "Hello, World!" > plaintext.txt
aws kms encrypt --key-id arn:aws:kms:us-east-1:123456789012:key/1234abcd-12ab-34cd-56ef-1234567890ab --plaintext fileb://plaintext.txt --grant-tokens <grant-token> --output text --query CiphertextBlob | base64 --decode > ciphertext.txt

# Use the grant to decrypt the ciphertext using the KMS key in Account A
aws kms decrypt --ciphertext-blob fileb://ciphertext.txt --grant-tokens <grant-token> --output text --query Plaintext | base64 --decode

# Use the grant to re-encrypt the ciphertext using the KMS key in Account A
aws kms re-encrypt --ciphertext-blob fileb://ciphertext.txt --destination-key-id arn:aws:kms:us-east-1:123456789012:key/1234abcd-12ab-34cd-56ef-1234567890ab --grant-tokens <grant-token> --output text --query CiphertextBlob | base64 --decode

# Use the grant to generate a data key using the KMS key in Account A
aws kms generate-data-key --key-id arn:aws:kms:us-east-1:123456789012:key/1234abcd-12ab-34cd-56ef-1234567890ab --key-spec AES_256 --grant-tokens <grant-token> --output text --query CiphertextBlob | base64 --decode

# Revoke the grant for the KMS key in Account A
aws kms revoke-grant --key-id arn:aws:kms:us-east-1:123456789012:key/1234abcd-12ab-34cd-56ef-1234567890ab --grant-id <grant-id>
```

## Allow other accounts to assume a role in your account to use a KMS key

This is a different approach to allow cross-account access to a KMS key. Instead of allowing an IAM user/role from another account to use the KMS key directly, you can allow an IAM user/role from another account to assume a role in your account to use the KMS key.

**Context:**

- Account A (123456789012) - KMS key owner
  - IAM role to use the KMS key
- Account B (210987654321) - IAM user to assume the role in Account A

**Steps:**

- Create a KMS key in Account A
- Use default key policy.

```json
{
  "Version": "2012-10-17",
  "Id": "key-default-1",
  "Statement": [
    {
      "Sid": "Allow use of the key",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::123456789012:root"
      },
      "Action": [
        "kms:Encrypt",
        "kms:Decrypt",
        "kms:ReEncrypt*",
        "kms:DescribeKey",
        "kms:GenerateDataKey*"
      ],
      "Resource": "*"
    }
  ]
}
```

- Create an IAM role in Account A and attach an IAM policy to the role to allow the role to use the KMS key.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "kms:Encrypt",
        "kms:Decrypt",
        "kms:ReEncrypt*",
        "kms:DescribeKey",
        "kms:GenerateDataKey*"
      ],
      "Resource": "arn:aws:kms:us-east-1:123456789012:key/1234abcd-12ab-34cd-56ef-1234567890ab"
    }
  ]
}
```

- Modify the trust policy of the IAM role in Account A to allow the IAM user in Account B to assume the role.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::210987654321:user/user-name"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

- Create an IAM user in Account B
- Attach an IAM policy to the IAM user in Account B to allow the user to assume the role in Account A.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "sts:AssumeRole",
      "Resource": "arn:aws:iam::123456789012:role/role-name"
    }
  ]
}
```

- Test the IAM user in Account B to assume the role in Account A to use the KMS key. Use CloudShell in the AWS Management Console in Account B to run the following commands:

```bash
# Assume the IAM role in Account A
aws sts assume-role --role-arn arn:aws:iam::123456789012:role/role-name --role-session-name test-session | jq -r '.Credentials | "export AWS_ACCESS_KEY_ID=\(.AccessKeyId)\nexport AWS_SECRET_ACCESS_KEY=\(.SecretAccessKey)\nexport AWS_SESSION_TOKEN=\(.SessionToken)"' | bash

# Describe the KMS key in Account A
aws kms describe-key --key-id arn:aws:kms:us-east-1:123456789012:key/1234abcd-12ab-34cd-56ef-1234567890ab

# Encrypt a plaintext file using the KMS key in Account A
echo "Hello, World!" > plaintext.txt
aws kms encrypt --key-id arn:aws:kms:us-east-1:123456789012:key/1234abcd-12ab-34cd-56ef-1234567890ab --plaintext fileb://plaintext.txt --output text --query CiphertextBlob | base64 --decode > ciphertext.txt

# Decrypt the ciphertext using the KMS key in Account A
aws kms decrypt --ciphertext-blob fileb://ciphertext.txt --output text --query Plaintext | base64 --decode

# Re-encrypt the ciphertext using the KMS key in Account A
aws kms re-encrypt --ciphertext-blob fileb://ciphertext.txt --destination-key-id arn:aws:kms:us-east-1:123456789012:key/1234abcd-12ab-34cd-56ef-1234567890ab --output text --query CiphertextBlob | base64 --decode

# Generate a data key using the KMS key in Account A
aws kms generate-data-key --key-id arn:aws:kms:us-east-1:123456789012:key/1234abcd-12ab-34cd-56ef-1234567890ab --key-spec AES_256 --output text --query CiphertextBlob | base64 --decode
```
