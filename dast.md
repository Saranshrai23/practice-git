# JAVA DAST – Security Analysis POC using OWASP ZAP for Salary API

---

# Author Information

| Author      | Created on | Version | Last updated by | Last edited on | L0 Reviewer | L1 Reviewer     | L2 Reviewer     |
| ----------- | ---------- | ------- | --------------- | -------------- | ----------- | --------------- | --------------- |
| Saransh Rai | 17-05-2026 | v1.0    | Saransh Rai     | 20-05-2026     | Anuj Jain   | Prashant Sharma | Piyush Upadhyay |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [Purpose](#2-purpose)
3. [Prerequisites](#3-prerequisites)
4. [Implementation Steps](#4-implementation-steps)

   * [4.1 Clone Repository](#41-clone-repository)
   * [4.2 Install Java](#42-install-java)
   * [4.3 Build Maven Artifact](#43-build-maven-artifact)
   * [4.4 Start Salary API](#44-start-salary-api)
   * [4.5 Verify Salary API](#45-verify-salary-api)
   * [4.6 Perform Health Check](#46-perform-health-check)
   * [4.7 Install OWASP ZAP](#47-install-owasp-zap)
   * [4.8 Start ZAP in Daemon Mode](#48-start-zap-in-daemon-mode)
   * [4.9 Verify OWASP ZAP](#49-verify-owasp-zap)
   * [4.10 Perform Spider Scan](#410-perform-spider-scan)
   * [4.11 Perform Active Scan](#411-perform-active-scan)
   * [4.12 Generate HTML Security Report](#412-generate-html-security-report)
   * [4.13 Open HTML Report in Browser](#413-open-html-report-in-browser)
   * [4.14 Understanding the HTML Report](#414-understanding-the-html-report)
5. [POC Validation](#5-poc-validation)
6. [POC Conclusion](#6-poc-conclusion)
7. [Documentation Reference](#7-documentation-reference)
8. [Contact Information](#8-contact-information)
9. [References](#9-references)

---

# 1. Introduction

Dynamic Application Security Testing (DAST) is a security testing methodology used to identify vulnerabilities in a running application by simulating real-world attacks.

Unlike Static Application Security Testing (SAST), DAST performs testing during runtime and helps detect issues such as:

* SQL Injection
* Cross-Site Scripting (XSS)
* Security Misconfiguration
* Missing Security Headers
* Information Disclosure

In this POC, **OWASP ZAP** is used to perform runtime security testing on the **Salary API** built using Java Spring Boot.

---

# 2. Purpose

The purpose of this POC is to perform **Dynamic Application Security Testing (DAST)** on the **Salary API** using **OWASP ZAP**.

This POC demonstrates:

* Running OWASP ZAP in daemon mode
* Performing Spider Scan
* Performing Active Scan
* Generating HTML security reports
* Understanding detected vulnerabilities
* Validating runtime security posture of the application

---

# 3. Prerequisites

| Tool / Requirement     | Purpose                             |
| ---------------------- | ----------------------------------- |
| OWASP ZAP              | Dynamic security testing tool       |
| Ubuntu / Linux Server  | Execution environment               |
| Java 17+               | Runtime requirement for Spring Boot |
| Maven                  | Build tool for Java application     |
| Salary API Application | Target application for scanning     |
| curl                   | Trigger ZAP API scans               |
| SCP                    | Secure report transfer              |
| Bastion Server         | Optional secure access layer        |

---

# 4. Implementation Steps

---

# <a name="41-clone-repository"></a> 4.1 Clone Repository

Clone the Salary API repository:

```bash
git clone https://github.com/OT-MICROSERVICES/salary-api.git
cd salary-api
```

<details>
<summary>Click to Expand Repository Clone Screenshot</summary>

<img width="1222" height="215" alt="image" src="https://github.com/user-attachments/assets/1b257b96-def0-4fe1-847f-3defbeae4bf2" />

</details>

---

# <a name="42-install-java"></a> 4.2 Install Java

Install Java 17:

```bash
sudo apt update
sudo apt install openjdk-17-jdk -y
java -version
```

### Expected Output

```bash
openjdk version "17"
```

<details>
<summary>Click to Expand Java Installation Screenshot</summary>

<img width="1917" height="672" alt="image" src="https://github.com/user-attachments/assets/7421cbea-6dba-451c-a81d-9b913a79b6ae" />

</details>

---

# <a name="43-build-maven-artifact"></a> 4.3 Build Maven Artifact

Build the Spring Boot application:

```bash
mvn clean install -DskipTests
```

### Meaning

| Command Part  | Explanation                     |
| ------------- | ------------------------------- |
| `mvn clean`   | Removes old build files         |
| `install`     | Builds and packages application |
| `-DskipTests` | Skips test execution            |

<details>
<summary>Click to Expand Build Screenshot</summary>

<img width="1891" height="837" alt="image" src="https://github.com/user-attachments/assets/f695e2f9-d091-457d-aea4-949819cabe38" />

</details>

---

# <a name="44-start-salary-api"></a> 4.4 Start Salary API

Run the Salary API:

```bash
nohup java -jar target/salary-0.1.0-RELEASE.jar --server.port=8082 > ~/salary.log 2>&1 &
```

## Command Breakdown

| Part                 | Meaning                                     |
| -------------------- | ------------------------------------------- |
| `nohup`              | Keeps process running after terminal closes |
| `java -jar`          | Runs executable JAR                         |
| `--server.port=8082` | Runs application on port 8082               |
| `> ~/salary.log`     | Stores logs in file                         |
| `2>&1`               | Redirects errors into same log              |
| `&`                  | Runs process in background                  |

<details>
<summary>Click to Expand Screenshot</summary>

<img width="1561" height="60" alt="image" src="https://github.com/user-attachments/assets/77cf0410-9fe2-4cef-84bf-acf973a36b70" />

</details>

---

# <a name="45-verify-salary-api"></a> 4.5 Verify Salary API

Verify whether application is listening on port 8082:

```bash
ss -tulnp | grep 8082
```

### Meaning

This command checks whether the Salary API is successfully running.

<details>
<summary>Click to Expand Screenshot</summary>

<img width="1232" height="96" alt="image" src="https://github.com/user-attachments/assets/3e41019c-25c2-4603-84fd-9a0207c2e5af" />

</details>

---

# <a name="46-perform-health-check"></a> 4.6 Perform Health Check

Run application health check:

```bash
curl http://localhost:8082/actuator/health
```

### Expected Output

```json
{"status":"UP"}
```

### Meaning

The Salary API is running successfully and reachable.

<details>
<summary>Click to Expand Screenshot</summary>

<img width="1902" height="111" alt="image" src="https://github.com/user-attachments/assets/56587c7c-4f9c-4eaa-b246-fd1ad6781a7b" />

</details>

---

# <a name="47-install-owasp-zap"></a> 4.7 Install OWASP ZAP

Install OWASP ZAP:

```bash
wget https://github.com/zaproxy/zaproxy/releases/download/v2.17.0/ZAP_2_17_0_unix.sh
chmod +x ZAP_2_17_0_unix.sh
./ZAP_2_17_0_unix.sh
zap.sh -version
```

### Expected Output

```bash
OWASP ZAP 2.17.0
```

<details>
<summary>Click to Expand OWASP ZAP Installation Screenshot</summary>

<img width="1906" height="832" alt="image" src="https://github.com/user-attachments/assets/290ea09d-b01b-4bc7-866f-dff48d44b837" />

</details>

---

# <a name="48-start-zap-in-daemon-mode"></a> 4.8 Start ZAP in Daemon Mode

Start OWASP ZAP in daemon mode:

```bash
zap.sh -daemon -host 127.0.0.1 -port 8090 -config api.disablekey=true > ~/zap.log 2>&1 &
```

## Command Breakdown

| Part                  | Meaning                   |
| --------------------- | ------------------------- |
| `zap.sh`              | Starts OWASP ZAP          |
| `-daemon`             | Runs without GUI          |
| `-host 127.0.0.1`     | Localhost access only     |
| `-port 8090`          | Runs ZAP API on port 8090 |
| `api.disablekey=true` | Disables API key          |
| `> ~/zap.log`         | Stores logs               |
| `&`                   | Runs in background        |

<details>
<summary>Click to Expand Screenshot</summary>

<img width="1572" height="87" alt="image" src="https://github.com/user-attachments/assets/8f040143-7b7c-4bde-833c-b7ed9b1d80c8" />

</details>

---

# <a name="49-verify-owasp-zap"></a> 4.9 Verify OWASP ZAP

Check whether ZAP is running:

```bash
ss -tulnp | grep 8090
```

Check ZAP version:

```bash
curl "http://localhost:8090/JSON/core/view/version/"
```

### Expected Output

```json
{"version":"2.17.0"}
```

### Meaning

OWASP ZAP daemon is successfully running.

<details>
<summary>Click to Expand Screenshot</summary>

<img width="1612" height="107" alt="image" src="https://github.com/user-attachments/assets/e0597fff-3ad7-45f7-8ec2-dc8ee0761df8" />

</details>

---

# <a name="410-perform-spider-scan"></a> 4.10 Perform Spider Scan

## What is Spider Scan?

Spider Scan acts like a crawler.

It discovers available URLs and endpoints inside the application.

It does not attack the application.

---

Run Spider Scan:

```bash
curl "http://localhost:8090/JSON/spider/action/scan/?url=http://localhost:8082/api/v1/salary/search/all"
```

### Expected Output

```json
{"scan":"0"}
```

### Meaning

Spider scan started successfully.

---

Check Spider Scan status:

```bash
curl "http://localhost:8090/JSON/spider/view/status/?scanId=0"
```

### Expected Output

```json
{"status":"100"}
```

### Meaning

Spider scan completed successfully.

<details>
<summary>Click to Expand Spider Scan Screenshot</summary>

<img width="1757" height="91" alt="image" src="https://github.com/user-attachments/assets/4c11ad39-d7ef-4bd8-b150-ed8a53f62f90" />

</details>

---

# <a name="411-perform-active-scan"></a> 4.11 Perform Active Scan

## What is Active Scan?

Active Scan performs actual security testing.

OWASP ZAP sends attack payloads to detect vulnerabilities like:

* SQL Injection
* Cross Site Scripting (XSS)
* Security Misconfiguration
* Information Disclosure
* Missing Security Headers

---

Run Active Scan:

```bash
curl "http://localhost:8090/JSON/ascan/action/scan/?url=http://localhost:8082/api/v1/salary/search/all"
```

### Expected Output

```json
{"scan":"0"}
```

### Meaning

Active Scan started successfully.

---

Check Active Scan status:

```bash
curl "http://localhost:8090/JSON/ascan/view/status/?scanId=0"
```

### Expected Output

```json
{"status":"100"}
```

### Meaning

Security testing completed successfully.

<details>
<summary>Click to Expand Active Scan Screenshot</summary>

<img width="1742" height="77" alt="image" src="https://github.com/user-attachments/assets/b760535d-9184-4a51-b4dc-da1ae182fc1e" />

</details>

---

# <a name="412-generate-html-security-report"></a> 4.12 Generate HTML Security Report

Generate OWASP ZAP HTML Report:

```bash
curl "http://localhost:8090/OTHER/core/other/htmlreport/" -o zap-report.html
```

## Command Breakdown

| Part                           | Meaning               |
| ------------------------------ | --------------------- |
| `curl`                         | Sends API request     |
| `OTHER/core/other/htmlreport/` | Generates HTML report |
| `-o zap-report.html`           | Saves output in file  |

---

## Verify Report Generation

```bash
ls -lh zap-report.html
```

### Example Output

```bash
-rw-rw-r-- 1 saransh saransh 23K May 20 19:43 zap-report.html
```

### Meaning

The OWASP ZAP report was successfully generated.

<details>
<summary>Click to Expand Screenshot</summary>

<img width="1467" height="171" alt="image" src="https://github.com/user-attachments/assets/e2bd7b8f-09eb-42d4-ab33-a996d7a3d6a7" />

</details>

---

# <a name="413-open-html-report-in-browser"></a> 4.13 Open HTML Report in Browser

Start lightweight Python web server:

```bash
cd ~/salary-api
python3 -m http.server 9000
```

## Meaning

| Part                     | Explanation                   |
| ------------------------ | ----------------------------- |
| `python3 -m http.server` | Starts lightweight web server |
| `9000`                   | Runs server on port 9000      |

Open report in browser:

```text
http://localhost:9000/zap-report.html
```

<details>
<summary>Click to Expand Screenshot</summary>

<img width="960" height="85" alt="image" src="https://github.com/user-attachments/assets/ece9be0c-1183-427c-98a2-6a73bab32ba6" />

<br>

<img width="1917" height="901" alt="image" src="https://github.com/user-attachments/assets/5be5c736-c2de-4484-8fb3-4bbbf61c4d77" />

<br>

<img width="1901" height="832" alt="image" src="https://github.com/user-attachments/assets/b483e1c6-590c-4957-89e9-6577a3119cbf" />

</details>

---

# <a name="414-understanding-the-html-report"></a> 4.14 Understanding the HTML Report

## Site Scanned

| Field              | Value                                   |
| ------------------ | --------------------------------------- |
| Target Application | `http://localhost:8082`                 |
| Report Access URL  | `http://localhost:9000/zap-report.html` |
| ZAP Version        | `2.17.0`                                |
| Generated On       | `20 May 2026`                           |

---

## Meaning

* `8082` → Actual Salary API scanned by ZAP
* `9000` → Temporary web server used to open report in browser

---

## Report Summary

| Risk Level | Alerts |
| ---------- | ------ |
| High       | 0      |
| Medium     | 0      |
| Low        | 1      |

---

## Meaning

* No High severity vulnerabilities found
* No Medium severity vulnerabilities found
* One Low severity vulnerability detected
* Overall application security posture was stable

---

## Vulnerability Found

| Vulnerability                | Risk Level |
| ---------------------------- | ---------- |
| Application Error Disclosure | Low        |

---

## Meaning

The application returned an internal server error (`HTTP 500`) during scanning, which may expose backend error information to users.

This is considered a Low severity vulnerability and can be fixed by hiding internal exception details in production environments.

---

# 5. POC Validation

| Validation Point             | Expected Result                  | Status |
| ---------------------------- | -------------------------------- | ------ |
| Java installed successfully  | `java -version` returns Java 17+ | Passed |
| Salary API started           | Application runs on port 8082    | Passed |
| Health endpoint accessible   | `/actuator/health` returns `UP`  | Passed |
| OWASP ZAP installed          | `zap.sh -version` works          | Passed |
| ZAP daemon started           | Running on port 8090             | Passed |
| Spider scan executed         | Scan ID returned                 | Passed |
| Active scan executed         | Scan completed successfully      | Passed |
| HTML report generated        | `zap-report.html` created        | Passed |
| Report accessible in browser | Opens on localhost:9000          | Passed |

---

# 6. POC Conclusion

The OWASP ZAP DAST scan completed successfully against the Salary API.

The generated HTML report confirmed:

* No High severity vulnerabilities
* No Medium severity vulnerabilities
* One Low severity issue related to Application Error Disclosure

This POC successfully demonstrated:

* Runtime application security testing
* Automated Spider and Active scanning
* Vulnerability detection using OWASP ZAP
* HTML report generation and analysis

---

# 7. Documentation Reference

| Topic                                                                                                                                                                                                       | Description                                                                                                                 |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| [JAVA DAST – Security Analysis Documentation using OWASP ZAP for Salary API](https://github.com/Snaatak-Infra-Titans/Documentations/blob/main/Application_CI_Design/Java_CI_Checks/DAST/Document/README.md) | Complete documentation covering DAST concepts, workflow, tools, comparison, advantages, best practices, and recommendations |

---

# 8. Contact Information

| Name        | Email                                                                           |
| ----------- | ------------------------------------------------------------------------------- |
| Saransh Rai | [saransh.rai.snaatak@mygurukulam.co](mailto:saransh.rai.snaatak@mygurukulam.co) |

---

# 9. References

| Topic                                                                   | Description                                |
| ----------------------------------------------------------------------- | ------------------------------------------ |
| [OWASP ZAP Documentation](https://www.zaproxy.org/docs/)                | Official OWASP ZAP documentation           |
| [OWASP Top 10](https://owasp.org/www-project-top-ten/)                  | Common web security vulnerabilities        |
| [Salary API Repository](https://github.com/OT-MICROSERVICES/salary-api) | Target Spring Boot application used in POC |
