# Principal Options in IAM Policies

## Principal

The `Principal` element is used in the `Statement` element of a policy to specify the entity that is allowed or denied access to a resource.
The Principal element is required in resource-based policies (like S3 bucket policies), but not required in IAM policies.

## Example

### AWS Account and Root User

```json
{
    "Principal": {
        "AWS": "123456789012"
    },
}
{
    "Principal": {
        "AWS": "arn:aws:iam::123456789012:root"
    }
}
```

### IAM Roles

```json
{
  "Principal": {
    "AWS": "arn:aws:iam::123456789012:role/role-name"
  }
}
```

### IAM Users

```json
{
  "Principal": {
    "AWS": "arn:aws:iam::123456789012:user/user-name"
  }
}
```

### IAM Groups

```json
{
  "Principal": {
    "AWS": "arn:aws:iam::123456789012:group/group-name"
  }
}
```

### IAM Role Sessions

IAM role sessions are temporary credentials that are assumed by a role.

```json
{
  "Principal": {
    "AWS": "arn:aws:sts::123456789012:assumed-role/role-name/session-name"
  }
}
```

SAML Provider

```json
{
  "Principal": {
    "Federated": "arn:aws:iam::123456789012:saml-provider/provider-name"
  }
}
```

Cognito Identity

```json
{
  "Principal": {
    "Federated": "cognito-identity.amazonaws.com"
  }
}
```

### Federated User Sessions STS

```json
{
  "Principal": {
    "AWS": "arn:aws:sts::123456789012:federated-user/user-name"
  }
}
```

### Service Principal

Service principals are AWS services that assume roles to access resources. Service principals are used to grant permissions to AWS services.

````json
{
  "Principal": {
    "Service": "service-name.amazonaws.com"
  }
}

```json
{
  "Principal": {
    "Service": ["ecs.amazonaws.com", "ec2.amazonaws.com", "s3.amazonaws.com"]
  }
}
````

### Canonical User ID

Canonical user ID is the unique identifier for an AWS account. It is used to grant permissions to a specific account.
When to use Canonical User ID:

- When you want to grant permissions to a specific account.
- When you want to grant permissions to a specific account that is not in your organization.

```json
{
  "Principal": {
    "CanonicalUser": "123456789012"
  }
}
```

### All Principals

```json
{
  "Principal": "*"
}
```

Or

```json
{
  "Principal": {
    "AWS": "*"
  }
}
```
