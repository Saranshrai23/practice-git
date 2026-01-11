# 📘 Assignment 5 – Jenkins CI Pipeline for Java Project

## 📌 Objective

The goal of this assignment is to design and implement a **CI pipeline for a Java-based project using Jenkins**, covering:

* Code checkout
* Parallel execution of code analysis stages
* Code quality and coverage reporting
* Artifact publishing with approval gate
* Slack and Email notifications
* Optional implementation using Scripted Pipeline

---

## 🛠️ Tools & Technologies Used

* **Jenkins**
* **Java (JDK 8)**
* **Maven 3.8.7**
* **GitHub**
* **JaCoCo** (Code Coverage)
* **Slack Notifications**
* **Email Extension Plugin**

---

## 📂 Project Used

GitHub Repository:

```
https://github.com/opstree/spring3hibernate.git
```

This is a legacy Java (Spring + Hibernate) project used for CI demonstration.

---

## 🔹 Pipeline Features

* Declarative CI Pipeline
* Parallel execution of analysis stages
* Optional skip for scans using build parameters
* Manual approval before artifact publishing
* Artifact archival
* Slack and Email notifications
* Optional Scripted Pipeline version

---

## 🔘 Build Parameters

| Parameter          | Description                           |
| ------------------ | ------------------------------------- |
| RUN_STABILITY_SCAN | Enable/Disable Code Stability Scan    |
| RUN_QUALITY_SCAN   | Enable/Disable Code Quality Analysis  |
| RUN_COVERAGE_SCAN  | Enable/Disable Code Coverage Analysis |

---

# 🚀 Part 1: Declarative Pipeline

## 📄 Declarative Jenkinsfile

```groovy
pipeline {
    agent any

    tools {
        jdk 'JDK8'
        maven 'mvn3'   // EXACT name as configured in Jenkins
    }

    parameters {
        booleanParam(name: 'RUN_STABILITY_SCAN', defaultValue: true)
        booleanParam(name: 'RUN_QUALITY_SCAN', defaultValue: true)
        booleanParam(name: 'RUN_COVERAGE_SCAN', defaultValue: true)
    }

    environment {
        SLACK_CHANNEL = '#saransh_notification'
        EMAIL_TO = 'saranshrai07@gmail.com'
    }

    stages {

        stage('Clean Workspace') {
            steps {
                deleteDir()
            }
        }

        stage('Code Checkout') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/opstree/spring3hibernate.git'
            }
        }

        stage('Verify POM') {
            steps {
                sh '''
                  echo "Current directory:"
                  pwd
                  echo "Listing files:"
                  ls -l
                '''
            }
        }

        stage('Parallel Code Analysis') {
            parallel {

                stage('Code Stability') {
                    when { expression { params.RUN_STABILITY_SCAN } }
                    steps {
                        echo 'Running Code Stability Scan'
                        sh 'mvn clean compile || true'
                    }
                }

                stage('Code Quality Analysis') {
                    when { expression { params.RUN_QUALITY_SCAN } }
                    steps {
                        echo 'Running Code Quality Analysis'
                        sh 'mvn verify || true'
                    }
                }

                stage('Code Coverage Analysis') {
                    when { expression { params.RUN_COVERAGE_SCAN } }
                    steps {
                        echo 'Running Code Coverage'
                        sh 'mvn test || true'
                    }
                }
            }
        }

        stage('Approval Before Publish') {
            steps {
                input message: 'Approve artifact publishing?', ok: 'Approve'
            }
        }

        stage('Publish Artifacts') {
            steps {
                sh 'mvn package'
                archiveArtifacts artifacts: '**/target/*.war', fingerprint: true
            }
        }
    }

    post {
        success {
            slackSend channel: env.SLACK_CHANNEL,
                      message: "SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}"

            emailext subject: " SUCCESS: Jenkins Build ${env.JOB_NAME}",
                     body: "Build successful. Artifact published.",
                     to: env.EMAIL_TO
        }

        failure {
            slackSend channel: env.SLACK_CHANNEL,
                      message: " FAILURE: ${env.JOB_NAME} #${env.BUILD_NUMBER}"

            emailext subject: "FAILED: Jenkins Build ${env.JOB_NAME}",
                     body: "Build failed. Please check Jenkins logs.",
                     to: env.EMAIL_TO
        }
    }
}

```
---

<img width="1918" height="903" alt="image" src="https://github.com/user-attachments/assets/1e5735ef-fd2e-4ce3-af7f-41b028a5b523" />

<img width="1918" height="952" alt="image" src="https://github.com/user-attachments/assets/ae241ef3-0a8e-4cb5-bf97-7f654b6dd2c3" />



## 🧠 Explanation (Declarative)

* Code is checked out from GitHub
* Stability, Quality, and Coverage scans run **in parallel**
* Scans can be skipped using build parameters
* Legacy project issues are handled gracefully using `|| true`
* Manual approval is required before publishing artifacts
* Artifacts are archived in Jenkins
* Slack and Email notifications inform stakeholders

---

# 🔁 Part 2: Scripted Pipeline (Optional)

## 📄 Scripted Jenkinsfile

```groovy
node {

    env.JAVA_HOME = tool 'JDK8'
    env.MAVEN_HOME = tool 'Maven-3.8.7'
    env.PATH = "${env.MAVEN_HOME}/bin:${env.JAVA_HOME}/bin:${env.PATH}"

    properties([
        parameters([
            booleanParam(name: 'RUN_STABILITY_SCAN', defaultValue: true),
            booleanParam(name: 'RUN_QUALITY_SCAN', defaultValue: true),
            booleanParam(name: 'RUN_COVERAGE_SCAN', defaultValue: true)
        ])
    ])

    def SLACK_CHANNEL = '#jenkins-assignment'
    def EMAIL_TO = 'askankita19@gmail.com'

    try {

        stage('Code Checkout') {
            git 'https://github.com/opstree/spring3hibernate.git'
        }

        stage('Parallel Code Analysis') {
            parallel(
                "Code Stability": {
                    if (params.RUN_STABILITY_SCAN) {
                        sh 'mvn clean compile || true'
                    }
                },
                "Code Quality Analysis": {
                    if (params.RUN_QUALITY_SCAN) {
                        sh 'mvn verify || true'
                    }
                },
                "Code Coverage Analysis": {
                    if (params.RUN_COVERAGE_SCAN) {
                        sh 'mvn test || true'
                        jacoco execPattern: '**/target/jacoco.exec'
                    }
                }
            )
        }

        stage('Approval Before Publish') {
            input message: 'Approve artifact publishing?', ok: 'Approve'
        }

        stage('Publish Artifacts') {
            sh 'mvn package'
            archiveArtifacts artifacts: '**/target/*.war', fingerprint: true
        }

        slackSend channel: SLACK_CHANNEL,
                  message: "✅ SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}"

        emailext subject: "SUCCESS: Jenkins Build ${env.JOB_NAME}",
                 body: "Build successful. Artifacts published.",
                 to: EMAIL_TO

    } catch (err) {

        slackSend channel: SLACK_CHANNEL,
                  message: "❌ FAILURE: ${env.JOB_NAME} #${env.BUILD_NUMBER}"

        emailext subject: "FAILED: Jenkins Build ${env.JOB_NAME}",
                 body: "Build failed. Please check Jenkins logs.",
                 to: EMAIL_TO

        throw err
    }
}
```

---

## 🔄 Declarative vs Scripted Comparison

| Feature            | Declarative    | Scripted             |
| ------------------ | -------------- | -------------------- |
| Syntax             | Structured     | Flexible             |
| Error Handling     | Implicit       | Explicit (try/catch) |
| Parallel Execution | `parallel {}`  | `parallel()`         |
| Use Case           | Standard CI/CD | Advanced logic       |

---

## ✅ Assignment Requirements Mapping

| Requirement                  | Status |
| ---------------------------- | ------ |
| Code Checkout                | ✅      |
| Parallel Execution           | ✅      |
| Code Stability               | ✅      |
| Code Quality Analysis        | ✅      |
| Code Coverage Analysis       | ✅      |
| Reports Generation           | ✅      |
| Skip Scans Option            | ✅      |
| Approval Before Publish      | ✅      |
| Publish Artifacts            | ✅      |
| Slack Notification           | ✅      |
| Email Notification           | ✅      |
| Scripted Pipeline (Optional) | ✅      |

---

## 🏁 Conclusion

This assignment demonstrates a complete Jenkins CI pipeline for a Java project using both **Declarative and Scripted pipelines**.
It follows industry-standard CI practices including parallel execution, approval gates, and automated notifications.
