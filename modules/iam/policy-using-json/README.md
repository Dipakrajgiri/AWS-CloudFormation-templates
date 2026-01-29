# IAM Managed Policy Module

## Purpose

This CloudFormation template creates a **reusable IAM Managed Policy**.  
It allows you to define the policy document dynamically and generates a standardized name using project and environment parameters.

This template is designed to be **modular and reusable** across multiple projects and environments.

---

## Resources Created

| Resource | Type | Description |
|----------|------|-------------|
| `IamManagedPolicy` | `AWS::IAM::ManagedPolicy` | Creates a single IAM Managed Policy with the provided JSON document and standardized naming |

---

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|---------|-------------|
| `Project` | `String` | Yes | Project name used in naming the managed policy |
| `Environment` | `String` | Yes | Environment name used in naming (e.g., dev, test, prod) |
| `PolicyName` | `String` | Yes | Base name of the IAM Managed Policy |
| `PolicyDocument` | `String` | Yes | JSON policy document as a string |
| `Description` | `String` | No | Optional description of the policy; defaults to empty string |

---

## Outputs

| Output | Description |
|--------|-------------|
| `PolicyArn` | ARN of the created IAM Managed Policy |
| `PolicyName` | Physical name of the IAM Managed Policy |

---

## Testing Example (Test Values)

This example can be used to **test the template immediately**. Replace the `TemplateURL` with your S3 or local path.

```yaml
Resources:
  TestPolicyStack:
    Type: AWS::CloudFormation::Stack
    Properties:
      TemplateURL: https://s3.amazonaws.com/my-cfn-templates/iam-managed-policy/template.yaml
      Parameters:
        Project: "acme"
        Environment: "dev"
        PolicyName: "S3ReadOnlyTest"
        PolicyDocument: '{"Version":"2012-10-17","Statement":[{"Effect":"Allow","Action":["s3:Get*","s3:List*"],"Resource":"*"}]}'
        Description: "Test read-only S3 access policy"


