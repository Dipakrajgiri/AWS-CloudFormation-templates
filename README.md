# AWS CloudFormation Infrastructure Module Library

## Introduction

This repository is a **foundational Infrastructure-as-Code (IaC) library** built using **AWS CloudFormation**. It provides **stable, reusable, production-grade infrastructure primitives** that can be safely consumed across **multiple AWS accounts, environments, and clients**.

This library is intentionally **opinionated, governed, and restrictive**. Its goal is to eliminate architectural drift, hidden dependencies, and environment coupling by enforcing **strict modular design principles**.

The templates in this repository are **NOT deployments**. They are **building blocks** that higher-level systems (CI/CD pipelines, orchestration repositories, or platform teams) compose and deploy.

---

## What This Repository Is

This repository is:

* A **reusable CloudFormation module library**
* A **foundation layer** for AWS infrastructure
* A collection of **single-responsibility templates**
* Designed for **long-term scalability and governance**
* Safe to use in **any AWS account or region**

Each module represents **one clear infrastructure responsibility** (for example: one IAM role, one VPC, one ALB, one security group).

All behavior, configuration, and dependencies are injected **explicitly via parameters**.

---

## What This Repository Is NOT

This repository is **not**:

* A deployment repository
* An environment-specific setup (dev, test, prod)
* A client-specific implementation
* A monolithic infrastructure template
* An orchestration or composition layer
* A place for business logic or workflows

No CI/CD logic, account assumptions, or environment branching belongs here.

---

## Design Philosophy

This library follows **strict Infrastructure-as-Code discipline**:

* **One template = one responsibility**
* **No hardcoded values of any kind**
* **Environment-agnostic by design**
* **Secure defaults are mandatory**
* **Optional behavior controlled only via boolean flags**
* **Predictable structure and naming**

Any template that violates these principles **must not be merged**.

---

## Intended Usage Model

This repository is designed to be consumed by:

* Platform engineering teams
* Central cloud teams
* CI/CD pipelines
* Environment-specific orchestration repositories

Typical usage pattern:

1. This repository defines **stable primitives**
2. A separate orchestration layer:

   * Passes parameters
   * Wires dependencies
   * Controls environment logic
3. Deployments happen **outside this repository**

This separation ensures:

* Reusability
* Predictability
* Safe multi-account usage
* Minimal blast radius

---

## Repository Structure Overview

```text
infra-core/
├── modules/          # Reusable CloudFormation modules ONLY
├── examples/         # Reference usage examples (non-production)
├── docs/             # Cross-cutting documentation
├── standards/        # Governance and enforcement rules
│   └── STANDARDS.md
└── README.md
```

Rules:

* `modules/` contains **only reusable templates**
* No client, account, or environment logic inside modules
* `examples/` are illustrative only, never production-ready
* No orchestration logic anywhere in this repository

---

## Module Philosophy

Each module:

* Lives in its own directory
* Contains exactly **one CloudFormation template**
* Solves **one infrastructure problem**
* Exposes **only required outputs**
* Declares **all dependencies explicitly**

Modules are designed to be:

* Composable
* Testable
* Replaceable
* Backward-compatible where possible

---

## Governance and Stability

This repository prioritizes **stability over speed**.

Breaking changes are avoided. When unavoidable, they must be:

* Explicit
* Documented
* Versioned externally

All templates must comply with the standards defined in:

```text
standards/STANDARDS.md
```

Any deviation from standards is considered a **design failure**, not an implementation detail.

---

## Audience

This library is intended for engineers who:

* Understand AWS fundamentals
* Value long-term maintainability
* Build infrastructure for multiple teams or clients
* Need predictable, reusable infrastructure components

This is **not** a beginner tutorial repository.

---

## Summary

This CloudFormation module library exists to provide:

* Strong architectural foundations
* Strict governance
* Reusable infrastructure primitives
* Safe multi-account deployments

If you are looking for **flexibility at deployment time**, that belongs in orchestration.

If you are looking for **stability and correctness**, you are in the right place.
