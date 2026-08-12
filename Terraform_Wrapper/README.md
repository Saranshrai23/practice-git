# Terraform Wrapper Code Documentation

| **Author**      | **Created on** | **Version** | **Last edited on** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| --------------- | -------------- | ----------- | ------------------ | --------------- | --------------- | --------------- |
| Saransh Rai     | 12-08-2026     | v1.0        | 12-08-2026         | Aniruth         | Aayush Verma    | Sandeep        |

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [Purpose](#2-purpose)
3. [Key Features](#3-key-features)
4. [Terraform Wrapper Code Architecture](#4-terraform-wrapper-code-architecture)
5. [Repository Structure](#5-repository-structure)
6. [Environment-Specific Deployment](#6-environment-specific-deployment)
7. [Configuration](#7-configuration)
8. [Variables Handling](#8-variables-handling)
9. [Secrets Handling](#9-secrets-handling)
10. [CI/CD Pipeline](#10-cicd-pipeline)
11. [CI/CD Stages](#11-cicd-stages)
12. [End-to-End Workflow](#12-end-to-end-workflow)
13. [Advantages & Disadvantages](#13-advantages--disadvantages)
14. [Troubleshooting](#14-troubleshooting)
15. [FAQs](#15-faqs)
16. [Conclusion](#16-conclusion)
17. [Contact Information](#17-contact-information)
18. [References](#18-references)

---

## 1. Introduction

Terraform Wrapper Code is an environment-specific configuration layer that uses reusable Terraform modules to provision infrastructure.

Instead of writing the complete Terraform resource configuration separately for Development, QA, UAT, and Production environments, common infrastructure logic is maintained inside reusable Terraform modules.

The wrapper code calls those modules and provides environment-specific values such as:

* Environment name
* Region
* VPC configuration
* Subnet configuration
* Instance type
* Cluster configuration
* Resource names
* Tags
* Scaling configuration
* Environment-specific settings

This approach provides a standardized, reusable, and maintainable way of managing infrastructure across multiple environments.

---

## 2. Purpose

The purpose of Terraform Wrapper Code is to separate **reusable infrastructure logic** from **environment-specific configuration**.

Terraform modules define **what infrastructure needs to be created**, while Terraform Wrapper Code defines **how that infrastructure should be configured for a particular environment**.

For example:

```text
Terraform Module
      +
Environment Values
      =
Environment Infrastructure
```

The same Terraform module can therefore be reused for:

```text
DEV
QA
UAT
PROD
```

without duplicating the complete Terraform resource code.

The wrapper code helps achieve:

* Code reusability
* Environment isolation
* Standardized infrastructure
* Easier maintenance
* Controlled deployments
* Better CI/CD integration

---

## 3. Key Features

| **Feature**                      | **Description**                                                                       |
| -------------------------------- | ------------------------------------------------------------------------------------- |
| Reusable Modules                 | Common infrastructure resources are maintained inside reusable Terraform modules.     |
| Environment Isolation            | Dev, QA, UAT, and Production can maintain separate configuration and Terraform state. |
| Centralized Infrastructure Logic | Infrastructure resource definitions are maintained in one module repository.          |
| CI/CD Integration                | Terraform execution can be automated using CI/CD pipelines.                           |
| Secure Secret Handling           | Sensitive values are injected securely instead of being committed to Git.             |
| Remote State Management          | Terraform state can be stored centrally using a remote backend.                       |
| Standardization                  | All environments use the same infrastructure module standards.                        |
| Easy Maintenance                 | Changes in common infrastructure logic can be managed centrally.                      |

---

## 4. Terraform Wrapper Code Architecture

Terraform implementation is divided into two major layers:

### Terraform Module

Terraform modules contain the actual infrastructure resource definitions.

Example:

```text
terraform-modules/
│
└── modules/
    │
    ├── terraform-aws-vpc/
    ├── terraform-aws-eks/
    ├── terraform-aws-ecr/
    ├── terraform-aws-rds/
    └── terraform-aws-iam/
```

A typical module may contain:

```text
terraform-aws-eks/
│
├── main.tf
├── variables.tf
├── outputs.tf
└── versions.tf
```

The module defines the actual cloud resources.

For example:

```hcl
resource "aws_eks_cluster" "main" {
  name     = var.cluster_name
  role_arn = var.cluster_role_arn

  vpc_config {
    subnet_ids = var.subnet_ids
  }
}
```

---

### Terraform Wrapper Code

Terraform Wrapper Code calls the reusable module and passes environment-specific configuration.

Example:

```hcl
module "eks" {
  source = "git::https://github.com/<organization>/<terraform-module-repository>.git//modules/terraform-aws-eks?ref=main"

  environment    = var.environment
  cluster_name   = var.cluster_name
  vpc_id         = var.vpc_id
  subnet_ids     = var.subnet_ids
  instance_types = var.instance_types
}
```

The basic architecture is:

```text
                    Reusable Terraform Modules
                              |
                              |
                 +------------+------------+
                 |                         |
                 v                         v
          DEV Wrapper Code           QA Wrapper Code
                 |                         |
                 v                         v
        DEV Infrastructure         QA Infrastructure
```

---

## 5. Repository Structure

A recommended Terraform Wrapper Code repository structure is:

```text
terraform-wrapper-code/
│
├── env/
│   │
│   ├── dev/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── terraform.tfvars
│   │   ├── providers.tf
│   │   ├── backend.tf
│   │   └── outputs.tf
│   │
│   └── qa/
│       ├── main.tf
│       ├── variables.tf
│       ├── terraform.tfvars
│       ├── providers.tf
│       ├── backend.tf
│       └── outputs.tf
│
├── .gitignore
│
└── README.md
```

Each environment maintains its own configuration while consuming the same reusable Terraform modules.

---

## 6. Environment-Specific Deployment

Terraform Wrapper Code allows different environments to use the same module with different configuration values.

### Development Environment

Example:

```hcl
environment = "dev"

aws_region = "ap-south-1"

cluster_name = "dev-eks-cluster"

instance_types = [
  "t3.medium"
]
```

### QA Environment

Example:

```hcl
environment = "qa"

aws_region = "ap-south-1"

cluster_name = "qa-eks-cluster"

instance_types = [
  "t3.large"
]
```

Both environments can use the same Terraform EKS module:

```text
                   terraform-aws-eks
                          |
                +---------+---------+
                |                   |
                v                   v
              DEV                  QA
                |                   |
         dev variables        qa variables
                |                   |
                v                   v
          DEV EKS Cluster      QA EKS Cluster
```

This ensures that infrastructure logic remains consistent while environment-specific configuration remains isolated.

---

## 7. Configuration

Terraform Wrapper Code normally contains multiple Terraform files, where each file has a specific responsibility.

### main.tf

`main.tf` is responsible for calling reusable Terraform modules.

Example:

```hcl
module "eks" {
  source = "git::https://github.com/<organization>/<terraform-module-repository>.git//modules/terraform-aws-eks?ref=main"

  environment  = var.environment
  cluster_name = var.cluster_name
  vpc_id       = var.vpc_id
  subnet_ids   = var.subnet_ids
}
```

---

### variables.tf

`variables.tf` defines the input variables accepted by the wrapper code.

Example:

```hcl
variable "environment" {
  description = "Environment name"
  type        = string
}

variable "cluster_name" {
  description = "Name of the EKS cluster"
  type        = string
}

variable "vpc_id" {
  description = "VPC ID where infrastructure will be deployed"
  type        = string
}

variable "subnet_ids" {
  description = "Subnet IDs used by the infrastructure"
  type        = list(string)
}
```

---

### terraform.tfvars

`terraform.tfvars` contains environment-specific non-sensitive values.

Example:

```hcl
environment = "dev"

aws_region = "ap-south-1"

cluster_name = "dev-eks-cluster"

instance_types = [
  "t3.medium"
]
```

Sensitive information such as passwords, tokens, client secrets, and access keys should not be stored inside `terraform.tfvars`.

---

### providers.tf

`providers.tf` defines Terraform provider requirements.

Example:

```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 6.0"
    }
  }
}

provider "aws" {
  region = var.aws_region
}
```

---

### backend.tf

`backend.tf` defines where Terraform state will be stored.

A remote backend is recommended for team-based deployments.

Example architecture:

```text
Terraform Pipeline
        |
        v
Remote Terraform Backend
        |
   +----+----+
   |         |
   v         v
DEV State   QA State
```

Different environments should maintain separate Terraform state.

Example:

```text
terraform-state/
│
├── dev/
│   └── terraform.tfstate
│
└── qa/
    └── terraform.tfstate
```

State isolation ensures that a Dev deployment cannot accidentally modify QA infrastructure.

---

## 8. Variables Handling

Terraform variables allow environment-specific configuration to be passed to reusable modules.

Variables can be divided into two categories:

### Non-Sensitive Variables

Examples include:

```text
environment
region
instance_type
cluster_name
vpc_cidr
subnet_cidr
resource_tags
min_size
max_size
desired_size
```

These variables can normally be stored inside environment-specific `.tfvars` files.

Example:

```hcl
environment   = "dev"
aws_region    = "ap-south-1"
instance_type = "t3.medium"
```

The flow is:

```text
terraform.tfvars
       |
       v
variables.tf
       |
       v
main.tf
       |
       v
Terraform Module
```

---

### Variable Precedence

Terraform can receive variables from multiple sources.

Common methods include:

```text
terraform.tfvars

*.auto.tfvars

-var

-var-file

TF_VAR_<variable_name>
```

Example:

```bash
terraform plan -var-file="dev.tfvars"
```

For environment-specific pipelines, a separate variable file can be maintained.

Example:

```text
dev.tfvars
qa.tfvars
uat.tfvars
prod.tfvars
```

---

## 9. Secrets Handling

Sensitive information should never be hardcoded inside Terraform source code.

Sensitive values include:

* AWS access keys
* Azure client secrets
* Git tokens
* Database passwords
* API keys
* Private keys
* Authentication tokens

Secrets should **not** be stored inside:

```text
main.tf
variables.tf
terraform.tfvars
Git repositories
Pipeline scripts
Module source URLs
```

---

### Recommended Secret Flow

```text
Secrets Manager / CI/CD Credential Store
                 |
                 v
          CI/CD Pipeline
                 |
                 v
         Environment Variable
                 |
                 v
             Terraform
```

For example:

```bash
export TF_VAR_database_password="$DATABASE_PASSWORD"
```

Terraform automatically maps:

```text
TF_VAR_database_password
```

to the Terraform variable:

```hcl
variable "database_password" {
  description = "Database password"
  type        = string
  sensitive   = true
}
```

---

### Private Git Module Authentication

If Terraform modules are stored inside a private Git repository, credentials should not be added directly to the module source.

The wrapper code should contain:

```hcl
module "eks" {
  source = "git::https://github.com/<organization>/<repository>.git//modules/terraform-aws-eks?ref=main"
}
```

Avoid configurations such as:

```text
https://username:token@github.com/organization/repository.git
```

The authentication should instead be handled by the CI/CD runner through an approved Git authentication mechanism.

Example:

```text
CI/CD Pipeline
       |
       | Git Authentication
       v
Private Git Repository
       |
       v
Terraform Module
       |
       v
terraform init
```

This ensures that Git credentials are not exposed inside Terraform code.

---

## 10. CI/CD Pipeline

Terraform deployments should be automated through a CI/CD pipeline instead of being manually executed from a developer machine.

The CI/CD pipeline provides:

* Repeatable deployments
* Automated validation
* Controlled infrastructure changes
* Approval mechanisms
* Auditability
* Reduced manual errors

A typical Terraform pipeline contains the following stages:

```text
Checkout
   |
   v
Terraform Init
   |
   v
Terraform Format
   |
   v
Terraform Validate
   |
   v
Terraform Plan
   |
   v
Approval
   |
   v
Terraform Apply
   |
   v
Validation
```

---

## 11. CI/CD Stages

### 11.1 Checkout

The pipeline first checks out the Terraform Wrapper Code repository.

Example:

```text
Git Repository
      |
      v
CI/CD Runner
```

The required environment directory is then selected.

Example:

```text
env/dev
```

or:

```text
env/qa
```

---

### 11.2 Terraform Init

Command:

```bash
terraform init
```

Purpose:

* Initializes the Terraform working directory.
* Downloads required providers.
* Downloads Terraform modules.
* Initializes the configured Terraform backend.
* Connects Terraform with remote state.

If modules are stored in private repositories, the CI/CD runner must have permission to access those repositories.

---

### 11.3 Terraform Format Check

Command:

```bash
terraform fmt -check -recursive
```

Purpose:

Checks whether Terraform files follow Terraform's standard formatting convention.

If incorrectly formatted Terraform files are found, the CI/CD pipeline can be configured to fail.

---

### 11.4 Terraform Validate

Command:

```bash
terraform validate
```

Purpose:

Validates the Terraform configuration.

It can identify issues such as:

* Invalid Terraform syntax
* Incorrect resource arguments
* Invalid module configuration
* Missing required configuration
* Incorrect variable references

---

### 11.5 Terraform Plan

Command:

```bash
terraform plan -out=tfplan
```

Purpose:

Terraform Plan compares the desired Terraform configuration with the existing infrastructure and displays the changes Terraform intends to perform.

Terraform Plan can display operations such as:

```text
+ Create

~ Update

- Destroy
```

Example output:

```text
Plan: 3 to add, 1 to change, 0 to destroy.
```

The generated Terraform plan should be reviewed before infrastructure changes are applied.

---

### 11.6 Approval

An approval stage can be configured before Terraform Apply.

Example:

```text
Terraform Plan
      |
      v
Manual Approval
      |
      v
Terraform Apply
```

Approval policies can differ depending on the environment.

Example:

| **Environment** | **Approval Strategy**          |
| --------------- | ------------------------------ |
| Dev             | Automatic or optional approval |
| QA              | Manual approval recommended    |
| UAT             | Manual approval                |
| Production      | Mandatory approval             |

The exact approval strategy should follow organizational requirements.

---

### 11.7 Terraform Apply

Command:

```bash
terraform apply tfplan
```

Purpose:

Applies the previously generated and reviewed Terraform execution plan.

Using the saved `tfplan` file ensures that the same changes reviewed during the Plan stage are applied.

---

### 11.8 Deployment Validation

After Terraform Apply completes successfully, the infrastructure should be validated.

Validation may include:

* Terraform output verification
* Resource status
* Network configuration
* IAM roles
* Security groups
* EKS cluster status
* Load balancer status
* Required endpoints
* Resource tags

Terraform outputs can be checked using:

```bash
terraform output
```

---

## 12. End-to-End Workflow

The complete Terraform Wrapper Code deployment flow is:

```mermaid
flowchart TD

    A[Developer] --> B[Update Terraform Wrapper Code]

    B --> C[Git Push / Pull Request]

    C --> D[CI/CD Pipeline Triggered]

    D --> E[Select Environment DEV / QA]

    E --> F[Checkout Wrapper Code]

    F --> G[Inject Required Credentials]

    G --> H[Terraform Init]

    H --> I[Download Terraform Providers]

    H --> J[Download Private Terraform Modules]

    I --> K[Terraform Format Check]

    J --> K

    K --> L[Terraform Validate]

    L --> M[Terraform Plan]

    M --> N[Review Terraform Plan]

    N --> O{Approval Required?}

    O -->|Yes| P[Manual Approval]

    O -->|No| Q[Terraform Apply]

    P --> Q

    Q --> R[Cloud Infrastructure]

    R --> S[DEV / QA Environment]

    S --> T[Post Deployment Validation]
```

### Workflow Explanation

```text
Developer
    |
    v
Modify Terraform Wrapper Code
    |
    v
Push Code to Git
    |
    v
CI/CD Pipeline Triggered
    |
    v
Select Environment
DEV / QA
    |
    v
Checkout Code
    |
    v
Inject Required Authentication
    |
    v
terraform init
    |
    v
Download Providers + Private Modules
    |
    v
terraform fmt -check
    |
    v
terraform validate
    |
    v
terraform plan
    |
    v
Review Plan
    |
    v
Approval
    |
    v
terraform apply
    |
    v
Infrastructure Created / Updated
    |
    v
Post Deployment Validation
```

---

## 13. Advantages & Disadvantages

| **Advantages**                                        | **Disadvantages**                                                                      |
| ----------------------------------------------------- | -------------------------------------------------------------------------------------- |
| Reusable Terraform modules reduce code duplication.   | Requires proper module version management.                                             |
| Environment-specific configuration remains separate.  | Repository structure may initially appear more complex.                                |
| Infrastructure standards can be maintained centrally. | Changes to shared modules can impact multiple environments if not versioned correctly. |
| Easy integration with CI/CD pipelines.                | CI/CD authentication must be configured correctly.                                     |
| Dev, QA, UAT, and Prod can use the same module.       | Remote state and locking must be carefully configured.                                 |
| Secrets can be managed securely outside source code.  | Poor secret handling can still expose sensitive information.                           |
| Infrastructure deployments become repeatable.         | Terraform state requires proper protection and backup.                                 |

---

## 14. Troubleshooting

| **Issue**                                | **Possible Cause**                             | **Solution**                                                                   |
| ---------------------------------------- | ---------------------------------------------- | ------------------------------------------------------------------------------ |
| `terraform init` fails                   | Backend or module access issue                 | Verify backend configuration, network access, and repository authentication.   |
| Private module cannot be cloned          | CI/CD runner does not have repository access   | Configure Git credentials or approved authentication on the CI/CD runner.      |
| `terraform validate` fails               | Invalid Terraform configuration                | Review Terraform syntax, variables, module inputs, and provider configuration. |
| `terraform plan` asks for missing values | Required variable was not supplied             | Verify `.tfvars`, environment variables, and variable definitions.             |
| Terraform state mismatch                 | Wrong backend/state file selected              | Verify environment-specific backend configuration.                             |
| Authentication error                     | Invalid or expired cloud credentials           | Verify CI/CD IAM role, service principal, or credential configuration.         |
| Terraform Apply fails                    | Insufficient cloud permissions                 | Verify IAM/RBAC permissions assigned to the CI/CD identity.                    |
| Incorrect environment resources modified | Wrong backend or variable file selected        | Validate environment selection before Terraform Plan and Apply.                |
| Secret displayed in output               | Terraform variable/output not marked sensitive | Mark sensitive Terraform variables and outputs with `sensitive = true`.        |

---

## 15. FAQs

### Q. What is Terraform Wrapper Code?

Terraform Wrapper Code is an environment-specific Terraform configuration layer that consumes reusable Terraform modules and passes the required values to them.

---

### Q. What is the difference between Terraform Module and Wrapper Code?

A Terraform module contains reusable infrastructure resource definitions.

Wrapper Code calls that module and provides environment-specific values.

In simple terms:

```text
Module = What infrastructure should be created?

Wrapper = What values should be used for this environment?
```

---

### Q. Why do we need Wrapper Code?

Wrapper Code allows the same Terraform module to be reused across multiple environments without duplicating Terraform resource definitions.

---

### Q. Can Dev and QA use the same Terraform module?

Yes.

Dev and QA can use the same module but provide different configuration values.

Example:

```text
Same EKS Module
      |
 +----+----+
 |         |
DEV       QA
```

---

### Q. Should Dev and QA use the same Terraform state?

No.

Each environment should use separate Terraform state to maintain proper isolation.

---

### Q. Where should secrets be stored?

Secrets should be stored in approved secret-management or CI/CD credential-management systems.

They should not be committed to Git.

---

### Q. Should Git tokens be added to the Terraform module source?

No.

Tokens should not be hardcoded inside Terraform module source URLs.

Authentication should be handled securely at the CI/CD runner or Git configuration level.

---

### Q. What happens during `terraform init`?

`terraform init`:

* Initializes the Terraform directory
* Downloads providers
* Downloads modules
* Initializes backend configuration
* Prepares Terraform for execution

---

### Q. What is the purpose of `terraform plan`?

`terraform plan` shows what Terraform intends to create, modify, or delete before any actual infrastructure changes are made.

---

### Q. Why do we save the Terraform Plan?

Saving Terraform Plan allows the reviewed plan to be applied later.

Example:

```bash
terraform plan -out=tfplan
```

Then:

```bash
terraform apply tfplan
```

---

### Q. Why is approval required before Terraform Apply?

Approval provides an additional control mechanism so infrastructure changes can be reviewed before they are applied.

It is particularly important for QA, UAT, and Production environments.

---

## 16. Conclusion

Terraform Wrapper Code provides a standardized approach for deploying infrastructure across multiple environments using reusable Terraform modules.

The Terraform modules contain reusable infrastructure definitions, while Wrapper Code provides environment-specific configuration.

The implementation can be summarized as:

```text
Terraform Modules
       +
Terraform Wrapper Code
       +
Environment-Specific Variables
       +
CI/CD Pipeline
       =
Automated Infrastructure Deployment
```

The CI/CD pipeline automates the complete Terraform lifecycle:

```text
Checkout
   |
   v
Init
   |
   v
Format
   |
   v
Validate
   |
   v
Plan
   |
   v
Approval
   |
   v
Apply
   |
   v
Validation
```

Using Terraform Wrapper Code provides:

* Reusability
* Environment isolation
* Standardization
* Secure secret management
* Automated infrastructure deployment
* Controlled infrastructure changes
* Improved maintainability

This architecture provides a scalable and maintainable way of managing Development, QA, UAT, and Production infrastructure using Terraform.

---

## 17. Contact Information

| **Name**        | **Email**         |
| --------------- | ----------------- |
| `<Author Name>` | `<email-address>` |

---

## 18. References

| **Link**                                                                                             | **Description**                                       |
| ---------------------------------------------------------------------------------------------------- | ----------------------------------------------------- |
| [Terraform Documentation](https://developer.hashicorp.com/terraform/docs)                            | Official Terraform documentation and concepts         |
| [Terraform Modules](https://developer.hashicorp.com/terraform/language/modules)                      | Official documentation for reusable Terraform modules |
| [Terraform Variables](https://developer.hashicorp.com/terraform/language/values/variables)           | Terraform input variable documentation                |
| [Terraform CLI - Init](https://developer.hashicorp.com/terraform/cli/commands/init)                  | Official documentation for `terraform init`           |
| [Terraform CLI - Plan](https://developer.hashicorp.com/terraform/cli/commands/plan)                  | Official documentation for `terraform plan`           |
| [Terraform CLI - Apply](https://developer.hashicorp.com/terraform/cli/commands/apply)                | Official documentation for `terraform apply`          |
| [Terraform Backends](https://developer.hashicorp.com/terraform/language/backend)                     | Official Terraform backend documentation              |
| [Terraform Sensitive Data](https://developer.hashicorp.com/terraform/language/manage-sensitive-data) | Guidelines for handling sensitive Terraform data      |
