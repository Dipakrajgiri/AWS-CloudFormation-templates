# IAM Role Module

## Purpose

This CloudFormation template creates a **reusable IAM Role module**.
It allows you to define a fully parameterized **trust (assume role) policy**, attach one or more **managed policy ARNs**, and optionally apply a **permissions boundary**.

The IAM Role name and tags are **automatically standardized** using `Project`, `Environment`, and `Name` parameters, making this template ideal for **multi-environment, multi-project infrastructures**.

This module is designed to be **modular, flexible, and production-ready**.

---

## Resources Created

| Resource  | Type             | Description                                                                                                                          |
| --------- | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| `IamRole` | `AWS::IAM::Role` | Creates a single IAM Role with a configurable trust policy, managed policies, optional permissions boundary, and standardized naming |

---

## Naming Convention

The physical IAM Role name is generated automatically:

```
<Project>-<Environment>-iam-role-<Name>
```

**Example:**

```
acme-dev-iam-role-lambda-exec
```

---

## Parameters

| Parameter                   | Type           | Required | Description                                                         |
| --------------------------- | -------------- | -------- | ------------------------------------------------------------------- |
| `Project`                   | `String`       | Yes      | Project name used for naming and tagging                            |
| `Environment`               | `String`       | Yes      | Environment name (e.g. dev, test, prod)                             |
| `Name`                      | `String`       | Yes      | Logical role name (e.g. app, lambda-exec, cicd)                     |
| `AssumeRolePolicyDocument`  | `String`       | Yes      | JSON trust policy document defining who can assume the role         |
| `AttachedPolicyArns`        | `List<String>` | Yes      | List of IAM Managed Policy ARNs to attach                           |
| `Path`                      | `String`       | No       | IAM role path (default: `/`)                                        |
| `MaxSessionDuration`        | `Number`       | No       | Maximum session duration in seconds (default: `3600`, max: `43200`) |
| `EnablePermissionsBoundary` | `String`       | No       | Whether to attach a permissions boundary (`true` / `false`)         |
| `PermissionsBoundaryArn`    | `String`       | No       | ARN of the permissions boundary policy                              |
| `ExtraTagKey`               | `String`       | No       | Optional additional tag key                                         |
| `ExtraTagValue`             | `String`       | No       | Optional additional tag value                                       |

---

## Conditions

| Condition                | Description                                                         |
| ------------------------ | ------------------------------------------------------------------- |
| `UsePermissionsBoundary` | Attaches a permissions boundary only if enabled and ARN is provided |
| `HasExtraTag`            | Adds an extra tag only if both key and value are provided           |

---

## Tags Applied

The following tags are always applied:

| Key           | Value              |
| ------------- | ------------------ |
| `Project`     | Project name       |
| `Environment` | Environment name   |
| `Name`        | Full IAM Role name |

An **additional custom tag** is added when `ExtraTagKey` and `ExtraTagValue` are provided.

---

## Outputs

| Output     | Description                   |
| ---------- | ----------------------------- |
| `RoleArn`  | ARN of the created IAM Role   |
| `RoleName` | Physical name of the IAM Role |

---

## Testing Example (Test Values)

This example can be used to **test the template immediately**.
Replace the `TemplateURL` with your S3 or local path.

```yaml
Resources:
  TestIamRoleStack:
    Type: AWS::CloudFormation::Stack
    Properties:
      TemplateURL: https://s3.amazonaws.com/my-cfn-templates/iam-role/template.yaml
      Parameters:
        Project: "acme"
        Environment: "dev"
        Name: "lambda-exec"
        AssumeRolePolicyDocument: >
          {
            "Version": "2012-10-17",
            "Statement": [
              {
                "Effect": "Allow",
                "Principal": {
                  "Service": "lambda.amazonaws.com"
                },
                "Action": "sts:AssumeRole"
              }
            ]
          }
        AttachedPolicyArns:
          - arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole
        MaxSessionDuration: 3600
        EnablePermissionsBoundary: false
```

---

## Use Cases

* Lambda execution roles
* ECS / EKS task roles
* CI/CD pipeline roles
* Cross-account IAM roles
* Standardized enterprise IAM role management
