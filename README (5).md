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

<img width="1212" height="792" alt="image" src="https://github.com/user-attachments/assets/c5534f47-55f7-405e-9900-4a18482e9d97" />

<img width="1747" height="790" alt="image" src="https://github.com/user-attachments/assets/da4f6ad0-d3b4-44ef-a68c-57aaf9475b3e" />


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
![Assignement1_task1_code](https://github.com/user-attachments/assets/b7d67375-597a-44e2-92d3-d022848f01e0)
![Assignement1_task1_code1](https://github.com/user-attachments/assets/701471a4-3e73-46cf-80c5-85bcd77f679b)



### * **Slack and email notification configuration**
<img width="1918" height="957" alt="image" src="https://github.com/user-attachments/assets/abf610cd-d5d6-4cd9-a955-c888ab8a60c9" />



### * **On failure**
<img width="1913" height="697" alt="image" src="https://github.com/user-attachments/assets/ab2cfff0-0769-4ac7-ae0c-ef727b4aeb61" />
<img width="1918" height="513" alt="image" src="https://github.com/user-attachments/assets/f9882bc1-b1e2-4597-9016-84b00e25f351" />
<img width="1912" height="952" alt="image" src="https://github.com/user-attachments/assets/7251a9fb-9c65-4b5a-9c72-115b96f89403" />
<img width="1400" height="312" alt="image" src="https://github.com/user-attachments/assets/ea21f876-d043-43bc-bdd8-6e59d94c78b8" />



### * **On success**
![Assignement1_task1](https://github.com/user-attachments/assets/0a457b9a-e728-42d4-961d-e9607532eac5)
<img width="1918" height="943" alt="image" src="https://github.com/user-attachments/assets/4d3a153c-bcc3-4691-a353-585d94c2f621" />
<img width="1553" height="256" alt="image" src="https://github.com/user-attachments/assets/a73e8593-679e-4d53-87d3-12064bab85a7" />
<img width="1547" height="646" alt="image" src="https://github.com/user-attachments/assets/bb5a5eb5-cddc-4e18-a41a-c55432384f36" />
<img width="1402" height="452" alt="image" src="https://github.com/user-attachments/assets/e0af886d-097b-44bc-8fa3-df8a9d80d31d" />



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
<img width="1918" height="970" alt="image" src="https://github.com/user-attachments/assets/e80c52c2-b141-4bf1-ba59-15b127846ef9" />


### * **Shell Script and Archiving Artifact**
<img width="1310" height="760" alt="image" src="https://github.com/user-attachments/assets/6b54ea8d-77a3-4a21-b4ed-4c52eb23b53c" />

### * **Slack and email notification configuration**
<img width="1337" height="703" alt="image" src="https://github.com/user-attachments/assets/8631c23f-7d35-406d-89cb-0b25d1e76e64" />


### * **Building with Parameter**
<img width="1918" height="662" alt="image" src="https://github.com/user-attachments/assets/ccf62cc8-3ec0-4cea-8b61-76186ffaee87" />

### * **Downstream job trigger configuration**
<img width="1895" height="762" alt="image" src="https://github.com/user-attachments/assets/10cb10aa-beef-4005-bf9b-16d66853015b" />


### * **Downstream job Shell Script**
<img width="1381" height="620" alt="image" src="https://github.com/user-attachments/assets/6d52aa4f-3817-4efc-88eb-d9c0d267e1a1" />


### * **On success (Downstream job)**
<img width="1887" height="966" alt="image" src="https://github.com/user-attachments/assets/33da312b-5aed-45a1-be4c-170873cb924e" />
<img width="1903" height="831" alt="image" src="https://github.com/user-attachments/assets/e827888a-f95c-4e9a-9fbf-fc089fc270d2" />


### * **On Failure**
<img width="1916" height="883" alt="image" src="https://github.com/user-attachments/assets/5512b771-6b32-43b9-8a45-628cb5c9ad46" />
<img width="1402" height="653" alt="image" src="https://github.com/user-attachments/assets/2d22587e-8c6d-4701-9db3-1a1cacd14420" />


### * **On Success**
<img width="1918" height="746" alt="image" src="https://github.com/user-attachments/assets/3764ede9-7d99-48c5-b295-9432a363685b" />

<img width="1402" height="653" alt="image" src="https://github.com/user-attachments/assets/8a2af2e0-b4fc-49cb-9a0b-2c02f86627a4" />


### * **On success(localhost)**
<img width="1918" height="920" alt="image" src="https://github.com/user-attachments/assets/f209158e-5269-4d0a-82d9-93e6b60a892a" />



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
