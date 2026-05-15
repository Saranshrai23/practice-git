<p align="left">
  <img width="190" height="191" alt="image" src="https://github.com/user-attachments/assets/717d70ec-91ab-4141-a105-9200a519f7e7" />
  <br/>
</p>

# Common Stack | Ansible | Playbook | CI Workflow Documentation

---

# Author Table

| Author      | Created on | Version | Last updated by | Last Edited On | L0 Reviewer | L1 Reviewer     | L2 Reviewer     |
| ----------- | ---------- | ------- | --------------- | -------------- | ----------- | --------------- | --------------- |
| Saransh Rai | 15-05-2026 | 1.0     | Saransh Rai     | 15-05-2026     | Anuj Jain   | Prashant Sharma | Piyush Upadhyay |

---

# Table of Contents

1. [Introduction](#1-introduction)<br>
2. [Purpose](#2-purpose)<br>
3. [What are Ansible Playbooks](#3-what-are-ansible-playbooks)<br>
4. [CI Checks in Ansible](#4-ci-checks-in-ansible)<br>
&nbsp;&nbsp;&nbsp;&nbsp;4.1 [Common CI Checks](#41-common-ci-checks)<br>
5. [CI Workflow for Playbooks](#5-ci-workflow-for-playbooks)<br>
&nbsp;&nbsp;&nbsp;&nbsp;5.1 [CI Workflow Diagram](#51-ci-workflow-diagram-click-to-expand)<br>
&nbsp;&nbsp;&nbsp;&nbsp;5.2 [Sample Jenkins Pipeline](#52-sample-jenkins-pipeline)<br>
6. [Best Practices](#6-best-practices)<br>
7. [Use Cases](#7-use-cases)<br>
8. [Troubleshooting](#8-troubleshooting)<br>
9. [Conclusion](#9-conclusion)<br>
10. [Contact Information](#10-contact-information)<br>
11. [References](#11-references)<br>

---


# 1. Introduction

Ansible Playbooks are YAML-based configuration files used to automate infrastructure provisioning, configuration management, and application deployment. Integrating CI (Continuous Integration) checks with playbooks ensures that automation code is validated, consistent, and production-ready before execution.

---

# 2. Purpose

This document provides an overview of Ansible playbooks and explains how CI checks can be applied to validate playbooks, enforce standards, and prevent errors before deployment.

---

# 3. What are Ansible Playbooks

Ansible Playbooks are structured YAML files that define a series of tasks to be executed on managed nodes. They allow automation of repetitive tasks such as software installation, configuration updates, and system provisioning in a consistent and repeatable manner.

---

# 4. CI Checks in Ansible

CI checks are automated validations performed on playbooks before they are merged or executed. These checks ensure correctness, security, and adherence to best practices.

## &nbsp;&nbsp;&nbsp;4.1 Common CI Checks

| CI Check                     | Command / Validation                                 | Purpose                                                                        |
| ---------------------------- | ---------------------------------------------------- | ------------------------------------------------------------------------------ |
| Syntax Validation            | `ansible-playbook --syntax-check playbook.yml`       | Verifies playbook and YAML syntax correctness before execution                 |
| Linting                      | `ansible-lint playbook.yml`                          | Enforces Ansible best practices, coding standards, and quality checks          |
| YAML Validation              | `yamllint playbook.yml`                              | Validates YAML indentation, formatting, and structure                          |
| Dry Run Validation           | `ansible-playbook playbook.yml --check`              | Simulates playbook execution without making actual changes                     |
| Formatting & Standardization | `ansible-lint --fix --dry-run`                       | Identifies formatting and standardization improvements without modifying files |
| Dependency Checks            | `ansible-galaxy`, `requirements.yml`, `ansible-lint` | Validates required roles and collections are installed                         |
| Security Checks              | `ansible-lint`, `git-secrets`, `gitleaks`, `trivy`   | Detects secrets, insecure configs, vulnerabilities                             |

---

# 5. CI Workflow for Playbooks

The CI workflow for Ansible playbooks ensures that playbooks are validated before deployment or execution. When a developer pushes a playbook to the Git repository, the CI pipeline is automatically triggered using Jenkins, GitHub Actions, or another CI/CD tool. The pipeline then runs configured validation checks such as syntax validation, linting, YAML validation, dry run, formatting, dependency, and security checks.

## &nbsp;&nbsp;&nbsp;5.1 CI Workflow Diagram (Click to Expand)

<details>
<summary>Click to Expand CI Workflow Diagram</summary>

<br>

<img width="1010" height="381" alt="image" src="https://github.com/user-attachments/assets/c8c581b0-e878-4772-bae8-d8abbc006985" />


</details>

## &nbsp;&nbsp;&nbsp;5.2 Sample Jenkins Pipeline

```groovy
pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/example/ansible-playbook-repo.git'
            }
        }

        stage('YAML Validation') {
            steps {
                sh 'yamllint playbook.yml'
            }
        }

        stage('Syntax Check') {
            steps {
                sh 'ansible-playbook --syntax-check playbook.yml'
            }
        }

        stage('Ansible Lint') {
            steps {
                sh 'ansible-lint playbook.yml'
            }
        }

        stage('Dependency Checks') {
            steps {
                sh 'ansible-galaxy install -r requirements.yml'
            }
        }

        stage('Security Checks') {
            steps {
                sh 'gitleaks detect --source .'
                sh 'trivy config .'
            }
        }

        stage('Dry Run') {
            steps {
                sh 'ansible-playbook playbook.yml --check'
            }
        }
    }
}
```

---

# 6. Best Practices

| Practice               | Description                               |
| ---------------------- | ----------------------------------------- |
| Use ansible-lint       | Ensures best practices and code quality   |
| Validate before commit | Run checks locally before pushing         |
| Avoid hardcoded values | Use variables and vaults                  |
| Modularize playbooks   | Use roles for better structure            |
| Use version control    | Maintain history and rollback             |
| Use Ansible Vault      | Protect sensitive credentials and secrets |
| Use proper task naming | Improves readability and troubleshooting  |
| Use CI/CD pipelines    | Automate validation before deployment     |

---

# 7. Use Cases

| Use Case                  | Description                                   |
| ------------------------- | --------------------------------------------- |
| Infrastructure Automation | Automate server provisioning                  |
| Configuration Management  | Maintain consistent system state              |
| CI/CD Pipelines           | Validate playbooks before deployment          |
| DevOps Standardization    | Enforce automation best practices             |
| Cloud Provisioning        | Automate AWS, Azure, and GCP resources        |
| Application Deployment    | Automate application installation and updates |

---

# 8. Troubleshooting

| Issue                  | Reason                              | Solution                                       |
| ---------------------- | ----------------------------------- | ---------------------------------------------- |
| Syntax error           | Invalid YAML                        | Run `--syntax-check`                           |
| Lint failure           | Code not following standards        | Fix using `ansible-lint`                       |
| Pipeline failure       | CI checks failed                    | Review logs and fix issues                     |
| Module not found       | Missing dependency                  | Install required roles/modules                 |
| YAML validation failed | Incorrect indentation               | Fix spacing and formatting                     |
| Permission denied      | Insufficient privileges             | Use proper sudo/root permissions               |
| Security scan failed   | Secrets or vulnerabilities detected | Remove secrets and fix insecure configurations |

---

# 9. Conclusion

CI checks for Ansible playbooks ensure reliable, error-free, and standardized automation, improving deployment quality and reducing failures in production. By integrating validation checks into CI/CD pipelines, organizations can maintain consistent infrastructure automation and improve deployment confidence.

---

# 10. Contact Information

| Name        | Email                                                                           |
| ----------- | ------------------------------------------------------------------------------- |
| Saransh Rai | [saransh.rai.snaatak@mygurukulam.co](mailto:saransh.rai.snaatak@mygurukulam.co) |

---

# 11. References

| Topic                                                | Description                        |
| ---------------------------------------------------- | ---------------------------------- |
| [Ansible Documentation](https://docs.ansible.com/)   | Official Ansible documentation     |
| [Ansible Lint](https://ansible-lint.readthedocs.io/) | Tool for checking playbook quality |
| [YAML Lint](https://yamllint.readthedocs.io/)        | YAML validation tool               |
