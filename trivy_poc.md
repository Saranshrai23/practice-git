# Python Dependency Scanning – TRIVY POC

---

# Author Information

| Author      | Created on | Version | Last updated by | Last edited on | L0 Reviewer | L1 Reviewer     | L2 Reviewer     |
| ----------- | ---------- | ------- | --------------- | -------------- | ----------- | --------------- | --------------- |
| Saransh Rai | 21-05-2026 | v1.0    | Saransh Rai     | 21-05-2026     | Anuj Jain   | Prashant Sharma | Piyush Upadhyay |

---

# Table of Contents

1. [Purpose](#1-purpose)
2. [Purpose](#2-purpose)
3. [Repositories Used](#3-repositories-used)
4. [Environment Setup](#4-environment-setup)
5. [Trivy Installation](#5-trivy-installation)
6. [Attendance API Scan](#6-attendance-api-scan)
7. [Notification Worker Scan](#7-notification-worker-scan)
8. [Vulnerability Summary](#8-vulnerability-summary)
9. [POC Validation](#9-poc-validation)
10. [Conclusion](#10-conclusion)
11. [Documentation Reference](#11-documentation-reference)
12. [Contact Information](#12-contact-information)
13. [References](#13-references)

---

# 1. Introduction

Trivy is an open-source security scanning tool used to identify vulnerabilities in dependencies, containers, filesystems, and source code projects. It helps development and DevSecOps teams detect outdated or vulnerable packages before applications are deployed into production environments.

In this POC, Trivy filesystem scanning is used to analyze Python-based microservices repositories and identify dependency-related security vulnerabilities.

---

# 2. Purpose

The purpose of this POC is to perform dependency vulnerability scanning on the Attendance API and Notification Worker repositories using Trivy filesystem scanning.

---

# 2. Repositories Used

| Service             | Repository                                                                                                                 |
| ------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| Attendance API      | [https://github.com/OT-MICROSERVICES/attendance-api.git](https://github.com/OT-MICROSERVICES/attendance-api.git)           |
| Notification Worker | [https://github.com/OT-MICROSERVICES/notification-worker.git](https://github.com/OT-MICROSERVICES/notification-worker.git) |

---

# 3. Environment Setup

The POC was executed on Ubuntu/WSL terminal. Initially, Trivy installation using Snap failed because the Snap store was not reachable.

```bash
sudo snap install trivy
```

Output:

```bash
error: unable to contact snap store
```

<details>
<summary>Click to View Snap Installation Error</summary>

<img width="900" alt="Snap installation error screenshot" src="PASTE_IMAGE_URL_HERE" />

</details>

Because Snap installation failed, Trivy was installed using the official APT repository method.

---

# 4. Trivy Installation

## <a name="41-update-packages"></a>    4.1 Update Packages

```bash
sudo apt update
sudo apt install wget apt-transport-https gnupg lsb-release -y
```

<details>
<summary>Click to View Package Update Output</summary>

<img width="900" alt="Package update screenshot" src="PASTE_IMAGE_URL_HERE" />

</details>

---

## <a name="42-add-trivy-gpg-key"></a>    4.2 Add Trivy GPG Key

```bash
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | \
gpg --dearmor | \
sudo tee /usr/share/keyrings/trivy.gpg > /dev/null
```

<details>
<summary>Click to View GPG Key Setup</summary>

<img width="900" alt="Trivy GPG key screenshot" src="PASTE_IMAGE_URL_HERE" />

</details>

---

## <a name="43-add-trivy-repository"></a>    4.3 Add Trivy Repository

```bash
echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main" | \
sudo tee /etc/apt/sources.list.d/trivy.list
```

Output:

```bash
deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb noble main
```

<details>
<summary>Click to View Repository Setup</summary>

<img width="900" alt="Trivy repository setup screenshot" src="PASTE_IMAGE_URL_HERE" />

</details>

---

## <a name="44-install-trivy"></a>    4.4 Install Trivy

```bash
sudo apt update
sudo apt install trivy -y
```

Output:

```bash
trivy is already the newest version (0.70.0).
```

<details>
<summary>Click to View Trivy Installation Output</summary>

<img width="900" alt="Trivy installation output screenshot" src="PASTE_IMAGE_URL_HERE" />

</details>

---

## <a name="45-verify-trivy-installation"></a>    4.5 Verify Trivy Installation

```bash
trivy --version
```

Output:

```bash
Version: 0.70.0
```

<details>
<summary>Click to View Trivy Version Output</summary>

<img width="900" alt="Trivy version screenshot" src="PASTE_IMAGE_URL_HERE" />

</details>

---

# 5. Attendance API Scan

## <a name="51-clone-attendance-api"></a>    5.1 Clone Attendance API Repository

```bash
cd ~
git clone https://github.com/OT-MICROSERVICES/attendance-api.git
```

Output:

```bash
Cloning into 'attendance-api'...
Receiving objects: 100% (167/167), done.
Resolving deltas: 100% (66/66), done.
```

<details>
<summary>Click to View Attendance API Clone Output</summary>

<img width="900" alt="Attendance API clone screenshot" src="PASTE_IMAGE_URL_HERE" />

</details>

---

## <a name="52-navigate-to-attendance-api"></a>    5.2 Navigate to Attendance API Directory

```bash
cd attendance-api
```

---

## <a name="53-run-attendance-api-trivy-scan"></a>    5.3 Run Trivy Filesystem Scan

```bash
trivy fs .
```

Scan Summary:

```bash
Target: poetry.lock
Type: poetry
Vulnerabilities: 18
Secrets: -
```

<details>
<summary>Click to View Attendance API Trivy Scan Output</summary>

<img width="900" alt="Attendance API Trivy scan output screenshot" src="PASTE_IMAGE_URL_HERE" />

</details>

---

## <a name="54-generate-attendance-api-report"></a>    5.4 Generate Attendance API Report

```bash
trivy fs --format table -o trivy_attendance_report.txt .
```

---

## <a name="55-view-attendance-api-report"></a>    5.5 View Attendance API Report

```bash
cat trivy_attendance_report.txt
```

Report Result:

```bash
Total: 18 (UNKNOWN: 0, LOW: 1, MEDIUM: 15, HIGH: 2, CRITICAL: 0)
```

<details>
<summary>Click to View Attendance API Generated Report</summary>

<img width="900" alt="Attendance API generated report screenshot" src="PASTE_IMAGE_URL_HERE" />

</details>

---

## <a name="56-attendance-api-scan-result"></a>    5.6 Attendance API Scan Result

### Target Scanned

```bash
poetry.lock
```

### Vulnerability Summary

| Severity | Count |
| -------- | ----: |
| Unknown  |     0 |
| Low      |     1 |
| Medium   |    15 |
| High     |     2 |
| Critical |     0 |
| Total    |    18 |

### Detected Vulnerable Libraries

| Library  | Installed Version | Vulnerability Examples                                                                                         | Severity    |
| -------- | ----------------: | -------------------------------------------------------------------------------------------------------------- | ----------- |
| flask    |             2.3.2 | CVE-2026-27205                                                                                                 | Low         |
| jinja2   |             3.1.2 | CVE-2024-22195, CVE-2024-34064, CVE-2024-56201, CVE-2024-56326, CVE-2025-27516                                 | Medium      |
| mistune  |             3.0.1 | CVE-2026-33079, CVE-2026-44708, CVE-2026-44896, CVE-2026-44897                                                 | High/Medium |
| pytest   |             7.4.0 | CVE-2025-71176                                                                                                 | Medium      |
| werkzeug |             2.3.6 | CVE-2024-34069, CVE-2023-46136, CVE-2024-49766, CVE-2024-49767, CVE-2025-66221, CVE-2026-21860, CVE-2026-27199 | High/Medium |

### Observation

The Attendance API scan detected vulnerabilities mainly in Python dependencies listed inside `poetry.lock`. The highest severity found was **High**, and the vulnerable packages should be upgraded to the fixed versions suggested by Trivy.

---

# 6. Notification Worker Scan

## <a name="61-clone-notification-worker"></a>    6.1 Clone Notification Worker Repository

```bash
cd ~
git clone https://github.com/OT-MICROSERVICES/notification-worker.git
```

Output:

```bash
Cloning into 'notification-worker'...
Receiving objects: 100% (13/13), done.
```

<details>
<summary>Click to View Notification Worker Clone Output</summary>

<img width="900" alt="Notification Worker clone screenshot" src="PASTE_IMAGE_URL_HERE" />

</details>

---

## <a name="62-navigate-to-notification-worker"></a>    6.2 Navigate to Notification Worker Directory

```bash
cd notification-worker
```

---

## <a name="63-run-notification-worker-trivy-scan"></a>    6.3 Run Trivy Filesystem Scan

```bash
trivy fs .
```

Scan Summary:

```bash
Target: requirements.txt
Type: pip
Vulnerabilities: 0
Secrets: -
```

<details>
<summary>Click to View Notification Worker Trivy Scan Output</summary>

<img width="900" alt="Notification Worker Trivy scan output screenshot" src="PASTE_IMAGE_URL_HERE" />

</details>

---

## <a name="64-generate-notification-worker-report"></a>    6.4 Generate Notification Worker Report

```bash
trivy fs --format table -o trivy_notification_report.txt .
```

---

## <a name="65-view-notification-worker-report"></a>    6.5 View Notification Worker Report

```bash
cat trivy_notification_report.txt
```

Report Result:

```bash
Target: requirements.txt
Type: pip
Vulnerabilities: 0
```

<details>
<summary>Click to View Notification Worker Generated Report</summary>

<img width="900" alt="Notification Worker generated report screenshot" src="PASTE_IMAGE_URL_HERE" />

</details>

---

## <a name="66-notification-worker-scan-result"></a>    6.6 Notification Worker Scan Result

### Target Scanned

```bash
requirements.txt
```

### Vulnerability Summary

| Severity | Count |
| -------- | ----: |
| Unknown  |     0 |
| Low      |     0 |
| Medium   |     0 |
| High     |     0 |
| Critical |     0 |
| Total    |     0 |

### Observation

The Notification Worker scan completed successfully and no dependency vulnerabilities were detected in `requirements.txt`.

---

# 7. Vulnerability Summary

| Service             | Target File      | Package Type | Vulnerability Status     | Total Findings |
| ------------------- | ---------------- | ------------ | ------------------------ | -------------: |
| Attendance API      | poetry.lock      | poetry       | Vulnerabilities detected |             18 |
| Notification Worker | requirements.txt | pip          | Clean                    |              0 |

---

# 8. POC Validation

| Validation Check                                   | Status |
| -------------------------------------------------- | ------ |
| Snap installation issue identified                 | Passed |
| Trivy installed using APT repository               | Passed |
| Trivy version verified as 0.70.0                   | Passed |
| Attendance API repository cloned successfully      | Passed |
| Attendance API filesystem scan executed            | Passed |
| Attendance API report generated                    | Passed |
| Notification Worker repository cloned successfully | Passed |
| Notification Worker filesystem scan executed       | Passed |
| Notification Worker report generated               | Passed |
| Vulnerability results reviewed                     | Passed |

---

# 9. Conclusion

The Trivy dependency scanning POC was completed successfully. Attendance API contained 18 dependency vulnerabilities, while Notification Worker showed no vulnerabilities. The vulnerable Attendance API packages should be upgraded as per Trivy fixed-version recommendations, and Trivy can be integrated into the CI/CD pipeline for continuous dependency security scanning.

---

# 10. Documentation Reference

| Document Name                                                    | Description                                                                                     |
| ---------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| [Dependency Scanning – TRIVY POC](PASTE_DOCUMENTATION_LINK_HERE) | POC documentation for scanning Attendance API and Notification Worker dependencies using Trivy. |

---

# 11. Contact Information

| Name        | Email                                                                           |
| ----------- | ------------------------------------------------------------------------------- |
| Saransh Rai | [saransh.rai.snaatak@mygurukulam.co](mailto:saransh.rai.snaatak@mygurukulam.co) |

---

# 12. References

| Reference                                                                         | Description                                                  |
| --------------------------------------------------------------------------------- | ------------------------------------------------------------ |
| [Trivy Official Documentation](https://trivy.dev/latest/docs/)                    | Official documentation for Trivy installation and scanning.  |
| [Trivy Filesystem Scanning](https://trivy.dev/latest/docs/target/filesystem/)     | Reference for scanning local project files using `trivy fs`. |
| [OWASP Dependency-Check Concept](https://owasp.org/www-project-dependency-check/) | Dependency vulnerability scanning concept reference.         |
| [OWASP Top 10](https://owasp.org/www-project-top-ten/)                            | Common web application security risk reference.              |
