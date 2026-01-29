# IAM Group Module

## Purpose

This CloudFormation template creates a **reusable IAM Group module**.
It allows you to define a single IAM Group and optionally attach **one or more managed policy ARNs**.

The IAM Group name is **automatically standardized** using `Project`, `Environment`, and `Name` parameters, making this template ideal for **multi-environment, multi-project infrastructures**.

This module is designed to be **modular, flexible, and production-ready**.

---

## Resources Created

| Resource   | Type              | Description                                                                       |
| ---------- | ----------------- | --------------------------------------------------------------------------------- |
| `IamGroup` | `AWS::IAM::Group` | Creates a single IAM Group with optional managed policies and standardized naming |

---

## Naming Convention

The physical IAM Group name is generated automatically:

```
<Project>-<Environment>-iam-group-<Name>
```

**Example:**

```
acme-dev-iam-group-admins
```

---

## Parameters

| Parameter            | Type                 | Required | Description                                                        |
| -------------------- | -------------------- | -------- | ------------------------------------------------------------------ |
| `Project`            | `String`             | Yes      | Project name used for naming                                       |
| `Environment`        | `String`             | Yes      | Environment name (e.g. dev, test, prod)                            |
| `Name`               | `String`             | Yes      | Logical group name (e.g. admins, dev-team)                         |
| `AttachedPolicyArns` | `CommaDelimitedList` | No       | Optional comma-separated list of IAM Managed Policy ARNs to attach |
| `Path`               | `String`             | No       | IAM group path (default: `/`)                                      |

---

## Conditions

| Condition             | Description                                                     |
| --------------------- | --------------------------------------------------------------- |
| `HasAttachedPolicies` | Attaches managed policies only if one or more ARNs are provided |

---

## Outputs

| Output      | Description                    |
| ----------- | ------------------------------ |
| `GroupName` | Physical name of the IAM Group |
| `GroupArn`  | ARN of the IAM Group           |

---

## Testing Example (Test Values)

This example can be used to **test the template immediately**.
Replace the `TemplateURL` with your S3 or local path.

```yaml
Resources:
  TestIamGroupStack:
    Type: AWS::CloudFormation::Stack
    Properties:
      TemplateURL: https://s3.amazonaws.com/my-cfn-templates/iam-group/template.yaml
      Parameters:
        Project: "acme"
        Environment: "dev"
        Name: "admins"
        AttachedPolicyArns: "arn:aws:iam::aws:policy/IAMReadOnlyAccess,arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess"
        Path: "/"
```

> **Important:** Do not include quotes around the comma-separated list in `AttachedPolicyArns` in the CloudFormation console; just use plain comma-separated ARNs.

---

## Use Cases

* Standardized IAM group creation for multiple environments
* Attaching AWS-managed or custom IAM policies to groups
* Enterprise-level access management
* Multi-project, multi-team IAM group management

---

## Notes / Best Practices

* Always use **AWS-managed policy ARNs** or valid custom managed policy ARNs.
* Avoid attaching AWS reserved policies to groups.
* For dynamic environments, use `Project` and `Environment` to standardize naming.
* Keep `AttachedPolicyArns` optional; the group can be created empty initially.
* For production, combine this module with a **centralized IAM policy library** for better governance.
