# POC – License Scanning using Trivy

---

## Document Details

| Author      | Created on | Version | Last updated by | Last edited on | L0 Reviewer | L1 Reviewer     | L2 Reviewer     |
| ----------- | ---------- | ------- | --------------- | -------------- | ----------- | --------------- | --------------- |
| Saransh Rai | 14-05-2026 | v1.0    | Saransh Rai     | 14-05-2026     | Anuj Jain   | Prashant Sharma | Piyush Upadhyay |

---

## Repository Scanned

[https://github.com/OT-MICROSERVICES/employee-api.git](https://github.com/OT-MICROSERVICES/employee-api.git)

---

# Table of Contents

1. [Introduction](#introduction)
2. [What is License Scanning?](#what-is-license-scanning)
3. [Why License Scanning is Important](#why-license-scanning-is-important)
4. [Purpose](#purpose)
5. [Workflow Diagram](#workflow-diagram)
6. [Different Tools](#different-tools)
7. [Tool Comparison](#tool-comparison)
8. [Advantages](#advantages)
9. [Prerequisites](#prerequisites)
10. [Clone Repository](#clone-repository)
11. [Install Trivy](#install-trivy)
12. [Run License Scan](#run-license-scan)
13. [Generate Reports](#generate-reports)
14. [Policy Enforcement](#policy-enforcement)
15. [Scan Validation](#scan-validation)
16. [Best Practices](#best-practices)
17. [Conclusion](#conclusion)
18. [Final Recommendation](#final-recommendation)
19. [Contact Information](#contact-information)
20. [References](#references)

---

# 1. Introduction

This document describes how to manually perform *license scanning* on the employee-api repository using *Trivy*. License scanning helps detect open-source dependency licenses and ensures compliance with internal organizational policies.

---

# 2. What is License Scanning?

License scanning is the process of identifying licenses associated with open-source libraries and dependencies used within an application. It helps organizations verify whether software components comply with internal security and legal policies.

---

# 3. Why License Scanning is Important

License scanning is important because some open-source licenses may impose restrictions on commercial usage, distribution, or modification. Performing regular license scans helps reduce legal risks, maintain compliance, and improve dependency governance in DevSecOps environments.

---

# 4. Purpose

This document explains the process of performing manual license scanning using Trivy on the employee-api repository. The purpose is to identify dependency licenses, validate compliance policies, and demonstrate how license scanning can support DevSecOps and CI/CD compliance workflows.

---

# 5. Workflow Diagram

```text
Developer Code
      │
      ▼
Repository Dependencies
      │
      ▼
Trivy License Scanner
      │
      ├── Detect Licenses
      ├── Validate Policies
      ├── Generate Reports
      └── Return Exit Codes
      │
      ▼
Compliance Validation / CI Pipeline
```
<img width="2104" height="1969" alt="mermaid-diagram (3)" src="https://github.com/user-attachments/assets/35332100-c84d-4e07-9614-ebf650cf4aa6" />

---

# 6. Different Tools

| Tool                   | Purpose                                  |
| ---------------------- | ---------------------------------------- |
| Trivy                  | License and vulnerability scanning       |
| FOSSA                  | Open-source license compliance           |
| Black Duck             | Enterprise software composition analysis |
| Snyk                   | Dependency and license scanning          |
| OWASP Dependency-Check | Dependency vulnerability analysis        |

---

# 7. Tool Comparison

| Tool                   | Open Source | License Scanning | CI/CD Integration | Ease of Use |
| ---------------------- | ----------- | ---------------- | ----------------- | ----------- |
| Trivy                  | Yes         | Yes              | Excellent         | Easy        |
| FOSSA                  | Partial     | Yes              | Excellent         | Moderate    |
| Black Duck             | No          | Yes              | Excellent         | Complex     |
| Snyk                   | Partial     | Yes              | Excellent         | Easy        |
| OWASP Dependency-Check | Yes         | Limited          | Good              | Moderate    |

---

# 8. Advantages

| Advantage          | Description                                |
| ------------------ | ------------------------------------------ |
| Legal Compliance   | Helps avoid usage of restricted licenses   |
| Lightweight        | Fast and simple scanning process           |
| CI/CD Integration  | Supports automated pipeline validation     |
| Audit Ready        | Generates compliance reports               |
| DevSecOps Friendly | Supports security and governance workflows |

---

# 9. Prerequisites

* Linux / Ubuntu / WSL
* Git installed
* Trivy installed
* Internet connection

---

# 10. Clone Repository

```bash
git clone https://github.com/OT-MICROSERVICES/employee-api.git
cd employee-api
```

---

# 11. Install Trivy

If not already installed:

```bash
sudo apt-get update
sudo apt-get install -y wget apt-transport-https gnupg lsb-release
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
echo "deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main" | sudo tee -a /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install -y trivy
```

## 11.1 Verify Installation

```bash
trivy --version
```

> **Screenshot Placeholder:** Insert screenshot showing successful Trivy installation and version output.

```text
[ Screenshot: Trivy Version Output ]
```

---

# 12. Run License Scan

⚠️ Important: In newer Trivy versions, use --scanners license

Run scan inside project directory:

```bash
trivy fs --scanners license .
```

> **Screenshot Placeholder:** Insert screenshot showing terminal output of license scanning results.

```text
[ Screenshot: Trivy License Scan Output ]
```

<img width="1698" height="465" alt="image" src="https://github.com/user-attachments/assets/de182322-73e6-4645-9191-5396dd29478b" />

This command:

* Scans project dependencies
* Detects license types
* Displays results in table format

---

# 13. Generate Reports

## 13.1 Table Report

```bash
trivy fs --scanners license -f table -o license-report.txt .
```

> **Screenshot Placeholder:** Insert screenshot showing generated table report.

```text
[ Screenshot: License Report Table Output ]
```

<img width="1889" height="582" alt="image" src="https://github.com/user-attachments/assets/0589d568-3a4a-459f-9387-7c1b91775480" />

## 13.2 JSON Report (Recommended for Audit)

```bash
trivy fs --scanners license -f json -o license-report.json .
```

> **Screenshot Placeholder:** Insert screenshot showing generated JSON report.

```text
[ Screenshot: JSON License Report ]
```

<img width="1919" height="920" alt="image" src="https://github.com/user-attachments/assets/6c6ea5e0-6e10-4f5e-985f-47d126741ae1" />

This report can be stored for compliance documentation.

---

# 14. Policy Enforcement

To fail build if any license issue is detected:

```bash
trivy fs --scanners license --exit-code 1 .
```

## 14.1 Exit Code Meaning

* 0 → No violation
* 1 → License detected (can be used to fail CI)

## 14.2 Validate Exit Code

```bash
echo $?
```

> **Screenshot Placeholder:** Insert screenshot showing exit code validation.

```text
[ Screenshot: Exit Code Validation ]
```

---

# 15. Scan Validation

| Scenario                 | Expected Result |
| ------------------------ | --------------- |
| Only MIT/Apache licenses | Scan Pass       |
| GPL license detected     | Exit Code 1     |
| No dependencies          | No issues       |
| JSON report generated    | File created    |

---

# 16. Best Practices

| Best Practice           | Description                                       |
| ----------------------- | ------------------------------------------------- |
| Perform regular scans   | Helps identify risky or restricted licenses early |
| Store JSON reports      | Maintains audit and compliance records            |
| Maintain license policy | Defines allowed and blocked licenses              |
| Review dependencies     | Prevents unauthorized package usage               |
| Integrate with CI/CD    | Enables automated compliance validation           |

## 16.1 Example License Policy

| License Type         | Status          |
| -------------------- | --------------- |
| MIT, Apache-2.0, BSD | Allowed         |
| LGPL, MPL            | Review Required |
| GPL-3.0              | Blocked         |

---

# 17. Conclusion

This POC successfully demonstrates how Trivy can be used for manual license scanning and compliance validation on the employee-api repository. The process is lightweight, easy to integrate, and suitable for both manual audits and CI/CD-based compliance enforcement.

# 18. Final Recommendation

Trivy is suitable for manual and CI-based license compliance due to:

* Open-source and free
* Lightweight
* Easy integration
* DevSecOps-friendly
* Supports license + vulnerability scanning

---

# 19. Contact Information

| Contact Type | Details                                                                         |
| ------------ | ------------------------------------------------------------------------------- |
| Name         | Saransh Rai                                                                     |
| Email        | [saransh.rai.snaatak@mygurukulam.co](mailto:saransh.rai.snaatak@mygurukulam.co) |

---

# 20. References

| Reference                                | Link                                                                                                         |
| ---------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| Trivy Official Documentation             | [https://trivy.dev](https://trivy.dev)                                                                       |
| Aqua Security Trivy GitHub Repository    | [https://github.com/aquasecurity/trivy](https://github.com/aquasecurity/trivy)                               |
| OT-MICROSERVICES Employee API Repository | [https://github.com/OT-MICROSERVICES/employee-api.git](https://github.com/OT-MICROSERVICES/employee-api.git) |
