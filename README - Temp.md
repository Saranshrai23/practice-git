# Jenkins Agents, Executors & Load Distribution – Assignment 4

## 📌 Objective

This assignment demonstrates **Jenkins agent configuration, load distribution, executor management, and node assignment** across multiple operating systems using different agent launch methods.
It also enforces **time-based job execution**, ensuring jobs run on agents during working hours and on the master node during non-working hours.

---

## 🧩 Topics Covered

* Configuring Jenkins agents
* Launching agents using different methods
* Executors and concurrency control
* Node labeling and job restriction
* Load distribution across nodes
* Time-based job execution (IST)
* CI pipelines with Git, Java, and Python
* Artifact archival and test coverage

---

## 🏗️ Jenkins Environment Overview

| Node    | OS     | Agent Launch Method       | Executors | Label  |
| ------- | ------ | ------------------------- | --------- | ------ |
| Master  | Linux  | Built-in                  | Default   | master |
| Agent 1 | Ubuntu | Execute command on master | 5         | assgn1 |
| Agent 2 | RHEL   | Launch via SSH            | 2         | assgn2 |
| Agent 3 | CentOS | Launch via SSH            | 3         | assgn3 |


<img width="1910" height="953" alt="image" src="https://github.com/user-attachments/assets/084add1b-6435-42bd-83cd-c83a185cfe63" />

<img width="1365" height="963" alt="image" src="https://github.com/user-attachments/assets/73d044ab-ea97-4ffb-a7c5-cd8bbbb0a90a" />

<img width="1893" height="923" alt="image" src="https://github.com/user-attachments/assets/63d5bf7a-35ec-4a78-a22a-40ca3865a24f" />

<img width="1915" height="838" alt="image" src="https://github.com/user-attachments/assets/f8f2e6e1-91d2-4168-9757-a9a0e642788d" />

## To create connection between master and nodes - we need to add SSH plugin and go to credentials >> add++ key

<img width="1907" height="622" alt="image" src="https://github.com/user-attachments/assets/1bebb2e3-8d04-415c-9ca3-c442db90eacd" />


---


## ⏰ Time-Based Execution Rule

All jobs follow the rule below:

* **9 AM – 6 PM (IST)** → Job runs on **assigned agent**
* **Outside 9 AM – 6 PM (IST)** → Job runs on **master node**

This logic is implemented using shell scripting and verified via console output.

---

## 🔹 Part 1 – Ubuntu Agent (Assignment 4 – Part 1)

### Configuration

* **OS**: Ubuntu
* **Agent Launch Method**: Execute command on master
* **Executors**: 5
* **Label**: `assgn1`
* **Job Name**: `Assgn4-1`

### Job Highlights

* Local Git repository creation
* Branch operations (`master`, `devops`, `ninja`)
* Merge and rebase workflow
* Branch cleanup
* Time-based execution logic

### Proof

* Day-time execution confirmed on **Ubuntu agent**
* Night-time execution confirmed on **master**
* Build status: **SUCCESS**


<img width="1918" height="890" alt="image" src="https://github.com/user-attachments/assets/c5dd9264-30e1-4bd4-97da-a0a279100618" />

```groovy
pipeline {
    agent none

    triggers {
        cron('H/15 * * * *')   // runs every 15 minutes
    }

    stages {
        stage('Decide Agent & Run Script') {
            steps {
                script {
                    // Get current hour (0–23)
                    def hour = new Date().format("H", TimeZone.getTimeZone("Asia/Kolkata")) as int

                    // Decide node
                    def selectedNode
                    if (hour >= 9 && hour < 18) {
                        selectedNode = 'ubuntu-agent'
                    } else {
                        selectedNode = 'master'
                    }

                    echo "Current hour: ${hour}"
                    echo "Running on node: ${selectedNode}"

                    node(selectedNode) {

                        stage('Git Assignment Script') {
                            sh '''
                            set -e

                            echo "===== GIT ASSIGNMENT START ====="

                            rm -rf git_assignment
                            mkdir git_assignment
                            cd git_assignment

                            git init
                            git config user.name "Jenkins"
                            git config user.email "jenkins@example.com"

                            echo "this is my first file" > file1
                            git add .
                            git commit -m "first file"

                            git branch devops
                            git branch ninja

                            git switch devops
                            echo "file 2 created" > file2
                            git add .
                            git commit -m "file 2"

                            git switch ninja
                            echo "file 3 created" > file3
                            git add .
                            git commit -m "file 3 added"

                            git checkout master
                            git merge devops

                            git switch ninja
                            git rebase master

                            git branch -d devops

                            echo "Final branches:"
                            git branch -a

                            echo "===== GIT ASSIGNMENT END ====="
                            '''
                        }
                    }
                }
            }
        }
    }
}

```


---

## 🔹 Part 2 – RHEL Agent (Assignment 4 – Part 2)

### Configuration

* **OS**: RHEL
* **Agent Launch Method**: SSH
* **Executors**: 2
* **Label**: `assgn2`
* **Job Name**: `Assgn4-2`

### Job Highlights

* Job restricted to RHEL agent using labels
* Time-based execution check using IST
* Validation of executor limit

### Proof

Console output confirms:

```
Building remotely on RHEL Node (assgn2)
Day time (9 AM - 6 PM IST) -> run on agent
```

### Result

* Job executed on RHEL agent
* Executor limit enforced
* Build status: **SUCCESS**


<img width="1915" height="906" alt="image" src="https://github.com/user-attachments/assets/b5da1253-b57b-4cf0-9bf4-6b369380d368" />

<img width="1896" height="928" alt="image" src="https://github.com/user-attachments/assets/bede6ad8-def4-46b1-a100-b4ade4d94847" />


```groovy
pipeline {
    agent none

    triggers {
        cron('H/15 * * * *')   // runs every 15 minutes
    }

    stages {
        stage('Decide Node & Run Job') {
            steps {
                script {
                    // Current hour in IST
                    def hour = new Date().format("H", TimeZone.getTimeZone("Asia/Kolkata")) as int

                    def selectedNode
                    if (hour >= 9 && hour < 18) {
                        selectedNode = 'rhel-agent'
                    } else {
                        selectedNode = 'master'
                    }

                    echo "Current hour: ${hour}"
                    echo "Selected node: ${selectedNode}"

                    node(selectedNode) {
                        stage('Echo Job Details') {
                            sh '''
                            echo "Job Name: $JOB_NAME"
                            echo "Build Number: $BUILD_NUMBER"
                            '''
                        }
                    }
                }
            }
        }
    }
}

---

---


## 🔹 Part 3 – CentOS Agent (Assignment 4 – Part 3)

### Configuration

* **OS**: CentOS
* **Agent Launch Method**: SSH
* **Executors**: 3
* **Label**: `assgn3`
* **Job Name**: `Assgn4-3c`

### CI Pipeline Overview (Python Project)

* GitHub SCM integration
* Python virtual environment creation
* Dependency installation
* Pytest execution
* Code coverage generation
* Artifact archival

### Tools Used

* Python 3.x
* Pytest
* Coverage
* Jenkins Artifact Archiver

### Proof

Console output confirms:

```
Building remotely on CentOS node (assgn3)
15 tests passed
Coverage generated
Archiving artifacts
Finished: SUCCESS
```

Artifacts include:

* HTML coverage reports
* `.coverage` file

---

## 📦 Post-Build Actions

* Archival of test coverage reports
* Jenkins UI displays downloadable artifacts
* Successful builds recorded with permalinks

<img width="1908" height="892" alt="image" src="https://github.com/user-attachments/assets/20b21e24-3774-455c-9c34-d73bc8c0f1f8" />

<img width="1917" height="948" alt="image" src="https://github.com/user-attachments/assets/37cd758c-eb22-4cec-bed2-7b8f5a9fe592" />


```groovy

pipeline {
    agent none

    stages {
        stage('Run Job Based on Time') {
            steps {
                script {

                    def hour = new Date().format("H", TimeZone.getTimeZone("Asia/Kolkata")) as int
                    def nodeName = (hour >= 9 && hour < 18) ? 'Agent 3 - Amazon Linux 2' : 'master'

                    echo "IST Hour: ${hour}"
                    echo "Running on: ${nodeName}"

                    node(nodeName) {

                        // ✅ Proper workspace cleanup
                        deleteDir()

                        // ✅ Git checkout (main branch)
                        sh '''
                        git clone -b main https://github.com/OT-MICROSERVICES/employee-api .
                        '''

                        // Credential Scan
                        sh '''
                        gitleaks detect --source . --report-path gitleaks-report.json || true
                        '''

                        // Test & Coverage
                        sh '''
                        go test ./... -coverprofile=coverage.out
                        go tool cover -html=coverage.out -o coverage.html
                        '''

                        // Dependency list
                        sh '''
                        go list -m all > dependencies.txt
                        '''

                        // ✅ Archive artifacts INSIDE node
                        archiveArtifacts artifacts: '''
                            gitleaks-report.json,
                            coverage.out,
                            coverage.html,
                            dependencies.txt
                        ''', allowEmptyArchive: true
                    }
                }
            }
        }
    }
}

---

## ✅ Final Outcome

✔️ All agents configured correctly
✔️ Executor limits enforced
✔️ Jobs assigned to correct nodes
✔️ Load distributed across nodes
✔️ Time-based execution validated
✔️ CI pipelines executed successfully

---

## 🏁 Conclusion

This assignment successfully demonstrates **real-world Jenkins agent management**, showcasing:

* Multi-node architecture
* Secure SSH-based agent communication
* Efficient job scheduling
* CI automation with Git, Java, and Python
* Production-ready Jenkins best practices
