# KMS Multi-Region Key

KMS multi-region keys are regional KMS keys that are replicated automatically to multiple AWS Regions. You can use multi-region keys to encrypt data in one AWS Region and decrypt it in a different AWS Region. This is useful when you have applications that run in multiple AWS Regions and need to share encrypted data between them.

When you create a multi-region key, KMS creates a primary key in the current AWS Region and a replica key in a different AWS Region. The primary key and replica key are exact copies of each other, and they are kept in sync automatically. When you use a multi-region key to encrypt data, KMS encrypts the data with the primary key and replicates the encrypted data to the replica key in the other AWS Region. When you use the replica key to decrypt the data, KMS decrypts the data with the replica key.

The following steps show how to use a multi-region key to encrypt data in one AWS Region and decrypt it in a different AWS Region:

1. Create a multi-region key in the primary AWS Region.
2. Encrypt data with the primary key in the primary AWS Region.
3. Decrypt data with the replica key in the replica AWS Region.
