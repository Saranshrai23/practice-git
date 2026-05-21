# Python Dependency Scanning – "TRIVY" POC

---

# Author Information

| Author      | Created on | Version | Last updated by | Last edited on | L0 Reviewer | L1 Reviewer     | L2 Reviewer     |
| ----------- | ---------- | ------- | --------------- | -------------- | ----------- | --------------- | --------------- |
| Saransh Rai | 21-05-2026 | v1.0    | Saransh Rai     | 21-05-2026     | Anuj Jain   | Prashant Sharma | Piyush Upadhyay |

---

# Table of Contents

1. [Purpose](#1-purpose)
2. [Scan Architecture](#2-scan-architecture)
3. [Environment Setup](#3-environment-setup)
4. [Trivy Scan Workflow](#4-trivy-scan-workflow)
5. [Attendance API Scan](#5-attendance-api-scan)
6. [Notification Worker Scan](#6-notification-worker-scan)
7. [Vulnerability Summary](#7-vulnerability-summary)
8. [POC Validation](#8-poc-validation)
9. [Conclusion](#9-conclusion)
10. [Documentation Reference](#10-documentation-reference)
11. [Contact Information](#11-contact-information)
12. [References](#12-references)


---

# 1. Purpose

The purpose of this POC is to demonstrate dependency vulnerability scanning using Trivy on Python-based services. The scan was performed on the Attendance API and Notification Worker to identify vulnerable dependencies and generate security reports using Trivy filesystem scanning.

---

# 2. Scan Architecture

<details>
<summary>Click to View Scan Architecture</summary>

<img width="420" height="338" alt="image" src="https://github.com/user-attachments/assets/93a15591-46ea-49ad-9a21-49525d6e628a" />

</details>

---

# 3. Environment Setup

## <a name="31-install-trivy"></a>     3.1 Install Trivy

```bash
sudo snap install trivy
```

---

## <a name="32-verify-installation"></a>     3.2 Verify Installation

```bash
trivy --version
```

Example Output:

```bash
Version: 0.52.2
```

<details>
<summary>Click to View Installation Output</summary>

<img width="952" height="152" alt="image" src="https://github.com/user-attachments/assets/ec665ff9-420c-44f7-8556-c70a6b072526" />

</details>

---

# 4. Trivy Scan Workflow

The dependency scan was performed using Trivy filesystem scanning.

## Workflow Steps

1. Navigate to project directory
2. Run Trivy filesystem scan
3. Analyze vulnerability findings
4. Generate scan report
5. Review detected vulnerabilities

---

# 5. Attendance API Scan

## <a name="51-navigate-to-project"></a>     5.1 Navigate to Project

```bash
cd ~/attendance
```

---

## <a name="52-run-trivy-scan"></a>     5.2 Run Trivy Scan

```bash
trivy fs .
```

<details>
<summary>Click to View Scan Output</summary>

<img width="1483" height="532" alt="image" src="https://github.com/user-attachments/assets/81d1327c-24f5-4270-8af9-98460083f933" />

</details>

---

## <a name="53-generate-scan-report"></a>     5.3 Generate Scan Report

```bash
trivy fs --format table -o trivy_attendance_report.txt .
```

<details>
<summary>Click to View Generated Report</summary>

<img width="1278" height="892" alt="image" src="https://github.com/user-attachments/assets/86187476-2f9c-40b3-9afa-9b60e84db060" />

<img width="1283" height="151" alt="image" src="https://github.com/user-attachments/assets/02d37463-08d2-47cd-84ea-ae9eba27c2b0" />

</details>

---

## <a name="54-scan-result"></a>     5.4 Scan Result

### Target Scanned

```bash
attendance_api/poetry.lock
```

### Vulnerability Summary

| Severity | Count |
| -------- | ----- |
| Low      | 1     |
| Medium   | 11    |
| High     | 1     |
| Critical | 0     |
| Total    | 13    |

### Detected Vulnerable Libraries

| Library  | Vulnerability  |
| -------- | -------------- |
| Flask    | CVE-2026-27205 |
| Jinja2   | CVE-2024-22195 |
| Werkzeug | CVE-2024-34069 |

---

# 6. Notification Worker Scan

## <a name="61-navigate-to-project"></a>     6.1 Navigate to Project

```bash
cd ~/notification-worker
```

---

## <a name="62-run-trivy-scan"></a>     6.2 Run Trivy Scan

```bash
trivy fs .
```

---

## <a name="63-generate-report"></a>     6.3 Generate Report

```bash
trivy fs --format table -o trivy_notification_report.txt .
```

<details>
<summary>Click to View Notification Worker Report</summary>

<img width="1600" height="406" alt="image" src="https://github.com/user-attachments/assets/63194a51-1839-4b3d-842a-00b21b3bd02f" />

<img width="1600" height="180" alt="image" src="https://github.com/user-attachments/assets/e11f1578-7667-4723-96f4-420902dca47f" />

</details>

---

## <a name="64-scan-result"></a>     6.4 Scan Result

### Target Scanned

```bash
requirements.txt
```

### Vulnerability Summary

| Severity | Count |
| -------- | ----- |
| Low      | 0     |
| Medium   | 0     |
| High     | 0     |
| Critical | 0     |

### Result

```bash
Clean (No vulnerabilities detected)
```

---

# 7. Vulnerability Summary

| Service             | Vulnerability Status        |
| ------------------- | --------------------------- |
| Attendance API      | 13 vulnerabilities detected |
| Notification Worker | No vulnerabilities detected |

Most vulnerabilities were caused by outdated Python dependencies.

---

# 8. POC Validation

| Validation Check                        | Status |
| --------------------------------------- | ------ |
| Trivy Installed Successfully            | Passed |
| Filesystem Scan Executed                | Passed |
| Report Generated Successfully           | Passed |
| Vulnerabilities Detected                | Passed |
| Notification Worker Security Validation | Passed |

---

# 9. Conclusion

Dependency vulnerability scanning was successfully performed using Trivy.

Key observations:

* Attendance API contains multiple vulnerable dependencies.
* Notification Worker dependencies are secure.
* Updating outdated libraries will reduce the application attack surface.
* Trivy can be integrated into CI/CD pipelines for continuous security monitoring.

---

# 10. Documentation Reference

| Document Name | Description |
|--------------|-------------|
| [Python Dependency Scanning – TRIVY Documentation](https://github.com/Snaatak-Infra-Titans/Documentations/blob/SCRUM-133-saransh/VCS_Implementation/Setup/Workflow/README.md) | Complete documentation covering Trivy workflow, comparison, best practices, and recommendations |

---

# 11. Contact Information

| Name        | Email                                                                           |
| ----------- | ------------------------------------------------------------------------------- |
| Saransh Rai | [saransh.rai.snaatak@mygurukulam.co](mailto:saransh.rai.snaatak@mygurukulam.co) |

---

# 12. References

| Reference                                                                        | Description                   |
| -------------------------------------------------------------------------------- | ----------------------------- |
| [https://trivy.dev/latest/docs/](https://trivy.dev/latest/docs/)                 | Official Trivy Documentation  |
| [https://owasp.org/www-project-top-ten/](https://owasp.org/www-project-top-ten/) | OWASP Security Best Practices |
| [https://docs.python.org/3/](https://docs.python.org/3/)                         | Python Official Documentation |

