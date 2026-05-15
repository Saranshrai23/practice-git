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

<img width="70%" height="1969" alt="mermaid-diagram (3)" src="https://github.com/user-attachments/assets/35332100-c84d-4e07-9614-ebf650cf4aa6" />

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

<details>
  <summary>Click to view image</summary>

  <img width="1007" height="191" alt="image" src="https://github.com/user-attachments/assets/ed266a42-b82c-4c80-ad15-138915c7b1ed" />

</details>

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

<details>
  <summary>Click to expand image</summary>

  <img width="1472" height="785" alt="image" src="https://github.com/user-attachments/assets/dce619e2-50f1-449c-a208-9335761dd65e" />

</details>


## &nbsp;&nbsp;&nbsp;&nbsp;11.1 Verify Installation

```bash
trivy --version
```

<details>
  <summary>Click to expand verification screenshot</summary>

  <img width="637" height="41" alt="image" src="https://github.com/user-attachments/assets/05fbf874-83f2-49a2-9e78-c8b42359dd1c" />

</details>

---

# 12. Run License Scan

⚠️ Important: In newer Trivy versions, use --scanners license

Run scan inside project directory:

```bash
trivy fs --scanners license .
```

<details>
  <summary>Click to expand all scan result screenshots</summary>

  <br>

  <img width="892" height="881" alt="image" src="https://github.com/user-attachments/assets/dc7b454e-e26c-48ac-8b60-bc0afec8e0c0" />

  <br><br>

  <img width="885" height="957" alt="image" src="https://github.com/user-attachments/assets/2ac83ac3-405d-4055-918d-6397bf4c312e" />

  <br><br>

  <img width="872" height="957" alt="image" src="https://github.com/user-attachments/assets/237f5d3e-6c96-44b4-89fd-223182a4cb9e" />

</details>


This command:

* Scans project dependencies
* Detects license types
* Displays results in table format

---

# 13. Generate Reports

## &nbsp;&nbsp;&nbsp;&nbsp;13.1 Table Report

```bash
trivy fs --scanners license -f table -o license-report.txt .
cat license-report.txt
```


<details>
  <summary>Click to expand table report screenshots</summary>

  <br>

  <img width="915" height="860" alt="image" src="https://github.com/user-attachments/assets/72542d5d-7c88-4cbc-bef4-a3664984f038" />

  <br><br>

  <img width="880" height="966" alt="image" src="https://github.com/user-attachments/assets/e135488e-efae-446e-a5c0-085be82b392b" />

  <br><br>

  <img width="867" height="971" alt="image" src="https://github.com/user-attachments/assets/ac60fd07-782a-48da-9d01-d567b16076f7" />

</details>


## &nbsp;&nbsp;&nbsp;&nbsp;13.2 JSON Report (Recommended for Audit)

```bash
trivy fs --scanners license -f json -o license-report.json .
cat license-report.json
```

<details>
  <summary>Click to expand JSON report screenshot</summary>

  <br>

  <img width="1331" height="912" alt="image" src="https://github.com/user-attachments/assets/e48b7ad2-0ca7-448e-bcb2-4af414030d01" />

</details>

This report can be stored for compliance documentation.

---

# 14. Policy Enforcement

To fail build if any license issue is detected:

```bash
trivy fs --scanners license --severity MEDIUM,HIGH,CRITICAL --exit-code 1 .
```

## &nbsp;&nbsp;&nbsp;&nbsp;14.1 Exit Code Meaning

* 0 → No violation
* 1 → License detected (can be used to fail CI)

## &nbsp;&nbsp;&nbsp;&nbsp;14.2 Validate Exit Code

```bash
echo $?
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

## &nbsp;&nbsp;&nbsp;&nbsp;16.1 Example License Policy

| License Type         | Status          |
| -------------------- | --------------- |
| MIT, Apache-2.0, BSD | Allowed         |
| LGPL, MPL            | Review Required |
| GPL-3.0              | Blocked         |

---

# 17. Conclusion

License scanning POC successfully completed using Trivy on the Employee API repository. Trivy scanned the Go module file and identified licenses used by direct and indirect dependencies, including MIT, BSD, Apache-2.0, ISC, and MPL-2.0. Most licenses were categorized as LOW severity notice licenses, while MPL-2.0 was marked as MEDIUM because it is a reciprocal license and may require additional compliance review.

## Final next step:

Review MPL-2.0 dependency with the team and add license scanning into CI/CD pipeline so every commit or merge request automatically checks license compliance.

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
