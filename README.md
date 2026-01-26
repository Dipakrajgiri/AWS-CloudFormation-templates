# CloudFormation Modular Infrastructure Framework

## Overview
This repository provides a standardized, reusable CloudFormation-based Infrastructure-as-Code framework for hosting multiple client workloads on AWS.

## Key Features
- Modular, component-level templates
- Multi-environment support
- Supports EC2, ECS, Fargate, Lambda, RDS, DynamoDB, S3, CloudFront
- Designed for long-term scalability

## How to Use
1. Choose required modules based on client needs
2. Configure environment parameters
3. Deploy using root-stack.yaml

## Deployment Order
Network → Security → Shared → Compute → Database → Edge → Observability

## Best Practices
- Never modify resources manually
- Always deploy via CI/CD
- Treat templates as shared libraries

## Ownership
Maintained by the Platform / DevOps Team
# AWS-CloudFormation-templates
