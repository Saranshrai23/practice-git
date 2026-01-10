# Jenkins Assignment 1 – Git Operations & Parameterized Job

## 📌 Objective

The goal of this assignment is to understand **Jenkins Freestyle Jobs**, **Git operations**, **Parameterized builds**, **Job chaining**, and **Slack & Email notifications** using Jenkins.

---

## 🛠️ Tools & Technologies Used

* Jenkins
* Git & GitHub
* Shell scripting
* Apache Web Server
* Slack (Incoming Webhook)
* Email (SMTP configuration)

---
## Screenshots:
### * **Global Configuration**
<img width="1772" height="762" alt="image" src="https://github.com/user-attachments/assets/19b57c65-ca2f-4d4d-be91-e97ba4527431" />

<img width="1908" height="965" alt="image" src="https://github.com/user-attachments/assets/56c5c727-a39d-413e-b2d3-dc393ceb8b7d" />


---

## 🔹 Part 1: Git Operations Using Jenkins

### 🎯 Requirement

Create a Jenkins job that performs the following Git operations:

1. Create a new branch
2. List all branches
3. Merge one branch into another
4. Rebase one branch with another
5. Delete a branch

👉 **Slack and Email notifications** must be sent if **any step fails**.

---

### 🧩 Jenkins Job Configuration (Part 1)

* **Job Type:** Freestyle Project
* **Source Code Management:** Git
* **Build Step:** Execute Shell

---

### 📜 Shell Script Logic

* Uses Git CLI commands
* Each operation is executed step-by-step
* Jenkins marks the build as **FAILED** if any Git command fails
* Post-build actions trigger **Slack & Email alerts**

---

### 📌 Sample Git Commands Used

```bash
git branch feature-1
git branch
git checkout main
git merge feature-1
git rebase main
git branch -d feature-1
```

---

### 🔔 Notifications

* ❌ **Failure:** Slack + Email sent immediately
* ✅ **Success:** Build completes without alerts

---
## Screenshots:
### * **Shell Script**
<img width="1854" height="1168" alt="Pasted image" src="https://github.com/user-attachments/assets/56a0f761-57e7-469c-8dc3-6393ea8edb4a" />
<img width="1854" height="1168" alt="Pasted image (2)" src="https://github.com/user-attachments/assets/efd656fa-ce5c-4776-91b6-502456b382f1" />

### * **Slack and email notification configuration**
<img width="1854" height="1168" alt="Pasted image (3)" src="https://github.com/user-attachments/assets/5321be54-f224-430e-b007-5cda5c899eaf" />


### * **On failure**
<img width="1816" height="534" alt="Pasted image (4)" src="https://github.com/user-attachments/assets/0d47d89c-e08b-47ba-9043-8c4dfca6a833" />
<img width="1816" height="659" alt="Pasted image (5)" src="https://github.com/user-attachments/assets/e9b32eac-9b08-4fa0-a36a-070bcd501950" />
<img width="867" height="837" alt="Pasted image (6)" src="https://github.com/user-attachments/assets/efe025d2-f6b3-4d5d-a623-fbb4a0f3ab90" />

### * **On success**
<img width="1195" height="1004" alt="Pasted image (7)" src="https://github.com/user-attachments/assets/22d1d8f3-4183-4ba1-9368-2f0e477aff09" />
<img width="1195" height="448" alt="Pasted image (8)" src="https://github.com/user-attachments/assets/8e7c488e-05c1-4413-9c6b-73b595580615" />
<img width="1195" height="828" alt="Pasted image (9)" src="https://github.com/user-attachments/assets/9d7c90b7-4274-4656-86a1-19bf565eb2f0" />

---

## 🔹 Part 2: Parameterized Build & Job Chaining

### 🎯 Requirement

Create two Jenkins jobs:

### ✅ Job 1: File Creation Job

* Accepts a **String Parameter**: `NINJA_NAME`
* Creates a file
* Writes content:

  ```
  <NINJA_NAME> from DevOps Ninja
  ```

---

### ✅ Job 2: File Publish Job

* Publishes the file created in Job 1
* Uses **Apache Web Server**
* Displays file content in browser

---

### 🔗 Job Trigger Condition

* Job 2 runs **automatically**
* Only when **Job 1 completes successfully**

---

### 🧩 Jenkins Job Configuration (Part 2)

#### Job 1

* **This project is parameterized**
* String parameter: `NINJA_NAME`
* Build step: Execute Shell

```bash
echo "$NINJA_NAME from DevOps Ninja" > ninja.txt
```

---

#### Job 2

* Triggered using **Build after other projects are built**
* Apache configured as web server
* File copied to `/var/www/html`

```bash
cp ninja.txt /var/www/html/
```

---

## 🔔 Notifications (Part 2)

| Scenario         | Notification  |
| ---------------- | ------------- |
| Any step fails   | Slack + Email |
| All jobs succeed | Slack + Email |

---
## Screenshots:

### * **Paramerterizing**
<img width="1854" height="1168" alt="Pasted image (2)" src="https://github.com/user-attachments/assets/c367e6d7-ecb1-4111-97e7-850b56dda602" />

### * **Shell Script and Archiving Artifact**
<img width="1854" height="1168" alt="Pasted image (3)" src="https://github.com/user-attachments/assets/fac45fc5-b0f2-45cf-b785-3e00ae84fa9f" />

### * **Slack and email notification configuration**
<img width="1854" height="1168" alt="Pasted image (4 1)" src="https://github.com/user-attachments/assets/a1b8c2ef-be3f-4353-8116-cea92a6adedf" />

### * **Building with Parameter**
<img width="967" height="652" alt="Pasted image" src="https://github.com/user-attachments/assets/66dce427-965f-4c7d-b965-0530a0190a01" />

### * **Downstream job trigger configuration**
<img width="1854" height="1168" alt="Pasted image (5)" src="https://github.com/user-attachments/assets/851f85d1-a39e-4c4f-9d6b-f10cbe37fab1" />

### * **Downstream job Shell Script**
<img width="1854" height="1168" alt="Pasted image (6)" src="https://github.com/user-attachments/assets/fc977c3d-6dba-4c27-8d79-62846a0b011e" />

### * **On success (Downstream job)**
<img width="1380" height="904" alt="Pasted image (7)" src="https://github.com/user-attachments/assets/b1945487-7376-4901-b458-410718e9cf02" />

### * **On success**
<img width="1380" height="457" alt="Pasted image (4)" src="https://github.com/user-attachments/assets/ff0cf4de-065a-42bb-9ecb-1c3d5192d243" />
<img width="959" height="852" alt="Pasted image (5 1)" src="https://github.com/user-attachments/assets/86414729-9c43-47b8-b80a-7d1c8f3698fe" />
<img width="959" height="448" alt="Pasted image (5 2)" src="https://github.com/user-attachments/assets/9d024d8e-63a8-4a43-a6a6-892cfa12120a" />

### * **On failure**
<img width="1375" height="489" alt="Pasted image (9)" src="https://github.com/user-attachments/assets/2acead0c-a019-4ac3-8183-d9304155ddb1" />
<img width="954" height="846" alt="Pasted image (8)" src="https://github.com/user-attachments/assets/dbfa64c1-a0d1-4645-aaad-bb1813b396a9" />
<img width="959" height="563" alt="Pasted image (10)" src="https://github.com/user-attachments/assets/237b80b8-6da4-4f8f-8d9e-79cea16d6080" />

### * **On success(localhost:8081)**
<img width="817" height="455" alt="image" src="https://github.com/user-attachments/assets/0e44f924-2592-4100-ab1d-d1e559148344" />


## ✅ Final Outcome

* Successfully automated Git operations using Jenkins
* Implemented parameterized builds
* Achieved job chaining
* Configured Slack & Email notifications
* Published build artifacts using Apache

---

## 🧠 Key Learnings

* Jenkins Freestyle job configuration
* Git automation via Jenkins
* Parameterized builds
* Inter-job dependency handling
* Monitoring builds using notifications

---

## 📌 Conclusion

This assignment provides hands-on experience with **real-world Jenkins automation scenarios**, commonly used in DevOps pipelines for CI/CD workflows.
