# NotAction

Use the `NotAction` element to specify the actions that you don't want to allow in a policy. The `NotAction` element is optional. If you don't include it, the policy allows all actions.

- You can't use the `NotAction` element with the `Action` element in a single statement.
- You can use the `NotAction` element with the `NotResource` element to specify what actions are allowed when a resource is not a specific value.

## Example

### NotAction with Allow

The following example shows a policy that allows all actions except `ec2:TerminateInstance` and `ec2:StopInstance`.

```json
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "NotAction": ["ec2:TerminateInstance", "ec2:StopInstance"],
    "Resource": "*"
  }
}
```

### NotAction with Deny

The following example shows a policy that denies all actions except `iam:*` with a condition if MFA is not present.

```json
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Deny",
    "NotAction": "iam:*",
    "Resource": "*",
    "Condition": {
      "BoolIfExists": {
        "aws:MultiFactorAuthPresent": "false"
      }
    }
  }
}
```

Deny everything outside of ap-southeast-1 region.

```json
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Deny",
    "NotAction": "*",
    "Resource": "*",
    "Condition": {
      "StringNotEquals": {
        "aws:RequestedRegion": "ap-southeast-1"
      }
    }
  }
}
```

Deny everything outside of ap-southeast-1 region and allow `s3:GetObject` action.

```json
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Deny",
    "NotAction": "s3:GetObject",
    "Resource": "*",
    "Condition": {
      "StringNotEquals": {
        "aws:RequestedRegion": "ap-southeast-1"
      }
    }
  }
}
```
