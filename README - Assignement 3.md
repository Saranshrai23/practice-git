# Jenkins CI Checks Using Freestyle Jobs – Assignment 3

## 📌 Objective

This assignment demonstrates how to implement **Continuous Integration (CI) checks using Jenkins Freestyle Jobs** for **multiple technology stacks**.
The goal is to validate code quality, security, and stability by running automated checks on **Python, GoLang, and Java APIs**.

Each job performs:

* Source code checkout from GitHub
* Security and quality checks
* Unit testing and code coverage
* Artifact generation and archival in Jenkins

---

## 🧩 Topics Covered

* Jenkins Freestyle Jobs
* GitHub SCM integration
* Credential scanning using Gitleaks
* Unit testing and code coverage
* Dependency analysis
* Artifact management
* Multi-language CI (Python, Go, Java)

---

## 🏗️ Jenkins Environment Overview

| Job Name | Technology     | Repository       | Branch |
| -------- | -------------- | ---------------- | ------ |
| Python   | Python / Flask | attendance-api   | main   |
| Go Job   | GoLang         | employee-api     | main   |
| Javaa    | Java / Maven   | spring3hibernate | master |

---

## 🔹 Part 1 – GoLang CI Job

### Configuration

* **Job Type**: Freestyle
* **Job Name**: `Go Job`
* **Language**: Go
* **Branch**: `main`
* **SCM**: GitHub (Public Repo)

Repository:

```
https://github.com/OT-MICROSERVICES/employee-api
```

---

### CI Checks Implemented

#### 🔐 Credential Scanning

Tool: **Gitleaks**

```
gitleaks detect --source . --report-path gitleaks-report.json
```

✔️ No secrets found
✔️ Security report generated

---

#### 🧪 Unit Testing & Coverage

```
go test ./... -coverprofile=coverage.out
go tool cover -html=coverage.out -o coverage.html
```

✔️ Tests executed successfully
✔️ HTML coverage report generated

---

#### 📦 Dependency Check

```
go list -m all > dependencies.txt
```

✔️ Dependency list generated for audit

---

### Artifacts Archived

* `gitleaks-report.json`
* `coverage.out`
* `coverage.html`
* `dependencies.txt`


<img width="1912" height="955" alt="image" src="https://github.com/user-attachments/assets/9d1ab03d-f6b2-44b7-aec4-79b2758a8063" />

<img width="1388" height="508" alt="image" src="https://github.com/user-attachments/assets/a84abf48-b7b7-46ba-97b9-4de012787c9e" />

<img width="1903" height="747" alt="image" src="https://github.com/user-attachments/assets/5ab23cb1-0b92-417b-92fa-49f6cf8c203c" />

---

### Result

✅ Build Successful
✅ Reports archived and accessible in Jenkins

---

## 🔹 Part 2 – Java CI Job

### Configuration

* **Job Type**: Freestyle
* **Job Name**: `Javaa`
* **Language**: Java
* **Build Tool**: Maven
* **Branch**: `master`

Repository:

```
https://github.com/opstree/spring3hibernate.git
```

---

### Environment Setup

* Java: OpenJDK 8
* Maven: 3.8.7

```
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export PATH=$JAVA_HOME/bin:$PATH
```

---

### CI Checks Implemented

#### 🔐 Credential Scanning

```
gitleaks detect --source . --report-format json --report-path gitleaks.json
```

✔️ Secrets detected and logged (learning/demo purpose)

---

#### 🧪 Build, Test & Coverage

```
mvn clean test package cobertura:cobertura
```

✔️ Code compiled successfully
✔️ Unit tests executed
✔️ Cobertura coverage report generated

---

#### 📊 Code Coverage Reporting

* Tool: **Cobertura**
* Report Path:

```
target/site/cobertura/coverage.xml
```

✔️ Coverage visible directly in Jenkins UI

---

### Artifacts & Reports

* `gitleaks.json`
* Cobertura coverage report
* Jenkins Coverage Dashboard

---

### Result

✅ Build Successful
✅ Coverage published
✅ Artifacts archived

---

## 🔹 Part 3 – Python CI Job

### Configuration

* **Job Type**: Freestyle
* **Job Name**: `Python`
* **Language**: Python (Flask)
* **Branch**: `main`

Repository:

```
https://github.com/OT-MICROSERVICES/attendance-api
```

---

### CI Pipeline Overview

#### 🧹 Workspace Cleanup

```
rm -rf .venv htmlcov .coverage
```

---

#### 🐍 Virtual Environment Setup

```
python3 -m venv .venv
source .venv/bin/activate
```

---

#### 📦 Dependency Installation

Application dependencies:

* Flask
* Redis
* Peewee
* psycopg2
* YAML

Testing tools:

```
pip install pytest pytest-cov coverage
```

---

#### 🧪 Unit Testing & Coverage

```
pytest \
 --maxfail=1 \
 --disable-warnings \
 --cov=. \
 --cov-report=html \
 --ignore=client/tests/test_postgres_conn.py \
 --ignore=client/tests/test_redis_conn.py
```

✔️ DB & Redis tests skipped to avoid external dependency failures
✔️ Coverage generated successfully

---

### Artifacts Archived

* `htmlcov/**`
* `.coverage`
* Individual HTML test reports

---

### Result

✅ Tests Passed
✅ Coverage Generated
✅ Artifacts Archived

---

## 📦 Artifact Management

* Jenkins local artifact storage
* Artifacts downloadable from Jenkins UI
* Reports retained per build

---

## ✅ Final Outcome

✔️ CI implemented for Python, Go, and Java
✔️ Credential scanning executed
✔️ Unit tests and coverage generated
✔️ Artifacts archived successfully
✔️ All jobs completed with **SUCCESS**

---

## 🏁 Conclusion

This assignment successfully demonstrates **multi-language CI automation using Jenkins Freestyle Jobs**, highlighting:

* Secure CI practices
* Automated testing and coverage
* Effective artifact management
* Real-world Jenkins CI workflows
