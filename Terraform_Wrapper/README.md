# Terraform Wrapper Code Documentation

| **Author**  | **Created on** | **Version** | **Last edited on** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| ----------- | -------------- | ----------- | ------------------ | --------------- | --------------- | --------------- |
| Saransh Rai | 12-08-2026     | v1.1        | 18-08-2026         | Aniruth         | Aayush Verma    | Sandeep         |

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

# 1. Introduction

Terraform Wrapper Code is an environment-specific layer that calls reusable Terraform modules and provides the required configuration values.

In the current project, Terraform is used to manage AWS infrastructure supporting an **internet-facing Load Balancer, frontend, three API services, Redis, PostgreSQL, ScyllaDB, and migration services**.

The current architecture does **not use EKS or Kubernetes**.

---

# 2. Purpose

The purpose of Terraform Wrapper Code is to separate reusable infrastructure code from environment-specific configuration.

```text
Reusable Terraform Module
        +
Environment Values
        =
Infrastructure
```

This reduces code duplication and makes infrastructure easier to manage across environments.

---

# 3. Key Features

| **Feature**           | **Description**                                    |
| --------------------- | -------------------------------------------------- |
| Reusable Modules      | Common infrastructure code is reused.              |
| Environment Isolation | Each environment maintains separate configuration. |
| CI/CD Integration     | Terraform is executed through Jenkins.             |
| Secure Secrets        | Sensitive values are kept outside source code.     |
| Standardization       | Environments follow the same module structure.     |

---

# 4. Terraform Wrapper Code Architecture

The project architecture consists of reusable Terraform modules and environment-specific wrapper code.

The application infrastructure follows this flow:

```text
                    Internet
                       |
                       v
            Internet-Facing Load Balancer
                       |
             +---------+---------+
             |                   |
             v                   v
         Frontend           3 API Services
                                  |
                     +------------+------------+
                     |            |            |
                     v            v            v
                   Redis      PostgreSQL    ScyllaDB
                   Cache       Database      Database
```

Database migrations are handled through:

* Liquibase
* Go-lang CLI

Terraform modules contain the reusable resource definitions, while wrapper code provides environment-specific values.

---

# 5. Repository Structure

Environment-specific Terraform code is maintained separately.

Example:

```text
terraform/
│
├── Env/
│   ├── Dev/
│   └── QA/
│
└── Modules/
```

Typical Terraform files include:

```text
main.tf
variables.tf
terraform.tfvars
providers.tf
backend.tf
outputs.tf
```

---

# 6. Environment-Specific Deployment

The same Terraform modules can be reused for different environments.

```text
              Terraform Modules
                     |
              +------+------+
              |             |
              v             v
          DEV Wrapper    QA Wrapper
              |             |
              v             v
           DEV Infra      QA Infra
```

Values such as region, VPC, subnets, resource names, instance configuration, and tags can differ between environments.

---

# 7. Configuration

The main Terraform files have specific responsibilities:

* **main.tf** – Calls reusable modules.
* **variables.tf** – Defines input variables.
* **terraform.tfvars** – Stores environment-specific non-sensitive values.
* **providers.tf** – Defines the AWS provider.
* **backend.tf** – Defines Terraform state configuration.
* **outputs.tf** – Returns required Terraform outputs.

---

# 8. Variables Handling

Terraform variables are used to provide environment-specific values.

Examples include:

```text
environment
region
vpc_id
subnet_ids
instance_type
resource_tags
```

Typical flow:

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

Sensitive values should not be stored in `.tfvars` files.

---

# 9. Secrets Handling

Sensitive values must not be hardcoded in Terraform or committed to Git.

Examples include:

* Database passwords
* AWS credentials
* Git credentials
* API keys

Secrets should be provided securely through Jenkins credentials or an approved secret-management mechanism.

```text
Credential Store
      |
      v
Jenkins Pipeline
      |
      v
Terraform
```

---

# 10. CI/CD Pipeline

Terraform deployment is automated through Jenkins.

The pipeline performs validation before making infrastructure changes.

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
```

---

# 11. CI/CD Stages

### 11.1 Checkout

Checks out the required Terraform repository and branch.

### 11.2 Terraform Init

```bash
terraform init
```

Initializes Terraform, providers, modules, and backend configuration.

### 11.3 Terraform Format Check

```bash
terraform fmt -check -recursive
```

Checks Terraform formatting.

### 11.4 Terraform Validate

```bash
terraform validate
```

Validates Terraform configuration.

### 11.5 Terraform Plan

```bash
terraform plan -out=tfplan
```

Shows planned infrastructure changes and stores the plan in `tfplan`.

### 11.6 Approval

The Terraform plan is reviewed before Apply.

### 11.7 Terraform Apply

```bash
terraform apply tfplan
```

Applies the reviewed Terraform plan.

### 11.8 Deployment Validation

After Apply, required AWS resources and application connectivity are verified.

---

# 12. End-to-End Workflow

```text
Developer
   |
   v
Update Wrapper Code
   |
   v
Push to Git
   |
   v
Jenkins Pipeline
   |
   v
Terraform Init
   |
   v
Format & Validate
   |
   v
Terraform Plan
   |
   v
Manual Approval
   |
   v
Terraform Apply
   |
   v
AWS Infrastructure
   |
   v
Internet-Facing Load Balancer
   |
   +----------------+
   |                |
   v                v
Frontend        API Services
                    |
          +---------+---------+
          |         |         |
          v         v         v
        Redis   PostgreSQL  ScyllaDB
                    ^
                    |
            Migration Services
         Liquibase / Go-lang CLI
```

---

# 13. Advantages & Disadvantages

| **Advantages**                 | **Disadvantages**                        |
| ------------------------------ | ---------------------------------------- |
| Reduces code duplication       | Requires proper module versioning        |
| Supports multiple environments | Incorrect variables can affect resources |
| Easy CI/CD integration         | Terraform state must be protected        |
| Standardized infrastructure    | Module changes require proper testing    |
| Easier maintenance             | Requires correct access management       |

---

# 14. Troubleshooting

| **Issue**                  | **Check**                                         |
| -------------------------- | ------------------------------------------------- |
| `terraform init` fails     | Backend and module access                         |
| `terraform validate` fails | Terraform syntax and variables                    |
| `terraform plan` fails     | Required variable values                          |
| `terraform apply` fails    | AWS permissions and Terraform plan                |
| ALB target unhealthy       | Target group, health check and application        |
| Database connection fails  | Network, security group and credentials           |
| Migration fails            | Database connectivity and migration configuration |

---

# 15. FAQs

### What is Terraform Wrapper Code?

It is an environment-specific Terraform layer that calls reusable modules and passes required values.

### Does the project use EKS?

No. The current infrastructure does not use EKS or Kubernetes.

### How does application traffic enter the infrastructure?

Traffic enters through an **internet-facing Load Balancer**, which routes requests to the frontend and API services.

### Which databases are used?

The project uses:

* PostgreSQL
* ScyllaDB

### What is Redis used for?

Redis is used as a caching layer.

### How are database migrations performed?

Migration services include:

* Liquibase
* Go-lang CLI

### Why is `terraform plan -out=tfplan` used?

It saves the generated plan so the same reviewed plan can be applied later.

---

# 16. Conclusion

Terraform Wrapper Code provides a reusable and environment-specific approach for managing AWS infrastructure.

For the current project, the infrastructure supports:

```text
Internet-Facing Load Balancer
        |
Frontend + 3 API Services
        |
Redis + PostgreSQL + ScyllaDB
        |
Liquibase / Go-lang CLI Migrations
```

Terraform deployment is controlled through Jenkins using Init, Format, Validate, Plan, Approval, and Apply stages.

---

# 17. Contact Information

| **Name**    | **Email**         |
| ----------- | ----------------- |
| Saransh Rai | `<email-address>` |

---

# 18. References

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
