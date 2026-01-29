# IAM User Module

## Purpose

This CloudFormation template creates a **reusable IAM User module**.
It allows you to define a single IAM User with optional:

* Managed Policy attachments
* Group membership
* Programmatic access via Access Key
* Permissions boundary

The IAM User name and tags are **automatically standardized** using `Project`, `Environment`, and `Name` parameters, making this template ideal for **multi-environment, multi-project infrastructures**.

This module is designed to be **modular, flexible, and production-ready**.

---

## Resources Created

| Resource       | Type                  | Description                                                                                          |
| -------------- | --------------------- | ---------------------------------------------------------------------------------------------------- |
| `IamUser`      | `AWS::IAM::User`      | Creates a single IAM User with optional group membership, managed policies, and permissions boundary |
| `IamAccessKey` | `AWS::IAM::AccessKey` | Optional Access Key for programmatic access (created only if `EnableAccessKey=true`)                 |

---

## Naming Convention

The physical IAM User name is generated automatically:

```
<Project>-<Environment>-iam-user-<Name>
```

**Example:**

```
acme-dev-iam-user-lambda
```

---

## Parameters

| Parameter                   | Type           | Required | Description                                                                    |
| --------------------------- | -------------- | -------- | ------------------------------------------------------------------------------ |
| `Project`                   | `String`       | Yes      | Project name used for naming and tagging                                       |
| `Environment`               | `String`       | Yes      | Environment name (e.g., dev, test, prod)                                       |
| `Name`                      | `String`       | Yes      | Logical user name (e.g., app, lambda, deploy)                                  |
| `Path`                      | `String`       | No       | IAM user path (default: `/`)                                                   |
| `GroupNames`                | `List<String>` | No       | Optional list of IAM Group names to attach this user to                        |
| `AttachedPolicyArns`        | `List<String>` | No       | Optional list of IAM Managed Policy ARNs to attach directly                    |
| `EnableAccessKey`           | `String`       | No       | Whether to create programmatic access (`true` / `false`, default: `false`)     |
| `EnablePermissionsBoundary` | `String`       | No       | Whether to attach a permissions boundary (`true` / `false`, default: `false`)  |
| `PermissionsBoundaryArn`    | `String`       | No       | ARN of the permissions boundary (required if `EnablePermissionsBoundary=true`) |
| `ExtraTagKey`               | `String`       | No       | Optional additional tag key                                                    |
| `ExtraTagValue`             | `String`       | No       | Optional additional tag value                                                  |

---

## Conditions

| Condition                | Description                                                                   |
| ------------------------ | ----------------------------------------------------------------------------- |
| `UsePermissionsBoundary` | Attaches a permissions boundary only if enabled and ARN is provided           |
| `HasExtraTag`            | Adds an extra tag only if both `ExtraTagKey` and `ExtraTagValue` are provided |
| `CreateAccessKey`        | Creates an Access Key only if `EnableAccessKey=true`                          |
| `AttachToGroups`         | Attaches the user to IAM Groups only if `GroupNames` is provided              |
| `AttachManagedPolicies`  | Attaches managed policies only if `AttachedPolicyArns` is provided            |

---

## Tags Applied

The following tags are always applied:

| Key           | Value              |
| ------------- | ------------------ |
| `Project`     | Project name       |
| `Environment` | Environment name   |
| `Name`        | Full IAM User name |

An **additional custom tag** is added when `ExtraTagKey` and `ExtraTagValue` are provided.

---

## Outputs

| Output            | Description                    |
| ----------------- | ------------------------------ |
| `UserName`        | Physical name of the IAM User  |
| `UserArn`         | ARN of the IAM User            |
| `AccessKeyId`     | Access Key ID (if created)     |
| `SecretAccessKey` | Secret Access Key (if created) |

---

## Testing Example (Test Values)

This example can be used to **test the template immediately**. Replace the `TemplateURL` with your S3 or local path.

```yaml
Resources:
  TestIamUserStack:
    Type: AWS::CloudFormation::Stack
    Properties:
      TemplateURL: https://s3.amazonaws.com/my-cfn-templates/iam-user/template.yaml
      Parameters:
        Project: "acme"
        Environment: "dev"
        Name: "lambda"
        Path: "/service/"
        GroupNames:
          - dev-iam-group
        AttachedPolicyArns:
          - arn:aws:iam::aws:policy/AdministratorAccess
        EnableAccessKey: true
        EnablePermissionsBoundary: false
        ExtraTagKey: "Team"
        ExtraTagValue: "DevOps"
```

---

## Use Cases

* User for Lambda or EC2 applications
* CI/CD deployment users
* Standardized enterprise IAM user management
* Users with temporary programmatic access
* Multi-environment or multi-project setups
