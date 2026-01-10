# Jenkins Assignment 2 – User Authentication & Authorization

## 📌 Assignment Overview

This assignment focuses on **Jenkins User Authentication and Authorization**.
The objective is to implement **role-based access control** for different teams and enable **Google SSO** for admin users.

📘 *Topics Covered:*

* Jenkins Authentication
* Jenkins Authorization
* Role-Based Authorization Strategy
* Google SSO Integration

---

## 🛠️ Tools & Technologies Used

* Jenkins
* Role-Based Authorization Strategy Plugin
* Google OAuth (SSO)
* Jenkins Freestyle Jobs

---

📌 **All screenshots should be stored inside the `screenshots/` folder**.

---

# 🔹 Part 1: User Authorization Using Role-Based Strategy

## 🏢 Organization Structure

The organization has **three teams**:

* Developer
* Testing
* DevOps

Additionally, there is **one Admin user** with full access.

---

## 👥 Users Created

### Developer Team

* Developer-1
* Developer-2

### Testing Team

* Tester-1
* Tester-2

### DevOps Team

* DevOps-1
* DevOps-2

### Administration

* Saransh (Admin-Full access)

📸 **Screenshots:**
### 📍 **Users**
 <img width="1916" height="897" alt="image" src="https://github.com/user-attachments/assets/8f6968c9-ede9-4fec-b4a4-43a34944afb1" />


---

## 🔐 Authorization Strategy Selected

After reviewing all available strategies, the **Role-Based Authorization Strategy** was selected because it allows:

* Fine-grained permission control
* Role-based job visibility
* Better scalability for organizations

### Authorization Strategies Reviewed

* Legacy Mode
* Project-Based Matrix
* Matrix-Based Security
* ✅ Role-Based Strategy (Implemented)

📸 **Screenshot:**
<img width="1917" height="803" alt="image" src="https://github.com/user-attachments/assets/c6b054fa-eda9-47ae-ab39-ef372884711f" />




---

## 🧩 Role Configuration

### 🔸 Developer Role

**Permissions:**

* Can see only `dev-*` jobs
* Can build jobs
* Can configure jobs
* Can view workspace

---

### 🔸 Tester Role

**Permissions:**

* Can see all `test-*` jobs
* Can view `dev-*` jobs
* Can build and configure test jobs
* Workspace access

---

### 🔸 DevOps Role

**Permissions:**

* Can see `devops-*`, `dev-*`, and `test-*` jobs
* Full job configuration access
* Workspace access

---

### 🔸 Admin Role

**Permissions:**

* Full Jenkins access (Overall Admin)

## 👁️ Jenkins Views Configuration

Separate views were created to restrict visibility:

* Developer View → Shows only `dev-*` jobs
* Testing View → Shows `test-*` and `dev-*` jobs
* DevOps View → Shows all jobs

📸 **Screenshots:**
### 📍 **Managing Roles**
<img width="1809" height="668" alt="Pasted image" src="https://github.com/user-attachments/assets/bc8f4011-1637-4b64-8d2e-c1077e554f4d" />
<img width="1809" height="668" alt="Pasted image (2)" src="https://github.com/user-attachments/assets/8a7cbfc8-2605-4e63-8613-a55e46cb0c7b" />

### 📍 **Assigning Roles**
<img width="901" height="687" alt="Pasted image (3)" src="https://github.com/user-attachments/assets/d976cc09-6414-4876-bb43-e4d0f1158a4c" />
<img width="901" height="687" alt="Pasted image (4)" src="https://github.com/user-attachments/assets/285bd3f3-f682-458b-a8d8-f1a798c86aa1" />

---

## 🏗️ Jenkins Jobs Created

A total of **9 Freestyle jobs** were created.

### Developer Jobs

* dev-1
* dev-2
* dev-3

### Testing Jobs

* test-1
* test-2
* test-3

### DevOps Jobs

* devops-1
* devops-2
* devops-3

Each job prints:

```
Job Name: <job-name>
Build Number: <build-number>
```

📸 **Screenshot:**
### 📍 **Jobs**
<img width="1854" height="1168" alt="Pasted image" src="https://github.com/user-attachments/assets/0820d0ac-53dd-4aca-8b5c-c30f686dd464" />

### 📍 **Console output**
<img width="901" height="687" alt="image" src="https://github.com/user-attachments/assets/7ad8afdf-87c8-4950-acee-25a6b0c36c57" />

📸 **Screenshot:**
### 📍 **DevOps view**

📍Dashboard:
<img width="1854" height="1168" alt="7" src="https://github.com/user-attachments/assets/11cc33f8-3bdf-4408-8fbb-11e8341403fe" />

📍Can build DevOps job 
<img width="1854" height="1168" alt="Pasted image (2)" src="https://github.com/user-attachments/assets/ff490448-c43e-4192-a740-d425f16175f7" />

📍Build Success
<img width="1854" height="1168" alt="Pasted image (3)" src="https://github.com/user-attachments/assets/0c0a5c49-54d8-434e-bc37-3829d136f41e" />

📍Build Success
<img width="901" height="687" alt="8" src="https://github.com/user-attachments/assets/263e395c-812e-4cea-88d1-314b0619363f" />

📍View mode of Tester via DevOps role
<img width="1854" height="1168" alt="Pasted image (4)" src="https://github.com/user-attachments/assets/b4d73ac1-e18d-45d7-b0da-c0c2badb1a56" />


---
# 🔹 Part 2: Google SSO Configuration

## 🎯 Requirement

Enable **Google Single Sign-On (SSO)** for **admin user access**.

---

## ⚙️ Configuration Steps

* Enabled **Google OAuth** under Jenkins Security Realm
* Configured:

  * Client ID
  * Client Secret
  * Authorized Redirect URI

---

## 🔑 Admin Login via SSO

* Admin user can now log in using Google credentials
* Successful authentication redirects to Jenkins dashboard

📸 **Screenshots:**
### 📍 **Google Cloud Console**
<img width="1854" height="1168" alt="Pasted image (2)" src="https://github.com/user-attachments/assets/4133d069-225a-4e24-8a04-44761d0d723d" />
<img width="1854" height="1168" alt="Pasted image" src="https://github.com/user-attachments/assets/12f5f732-0ba6-47e2-9788-e9b510d87074" />

### 📍 **Global Configuration**
<img width="1854" height="1168" alt="Pasted image (3)" src="https://github.com/user-attachments/assets/46e37545-98e3-4068-ba4c-fa79b1330eb4" />
<img width="1854" height="1168" alt="Pasted image (4)" src="https://github.com/user-attachments/assets/7a655fec-d2d6-462f-9ff3-6023acbcbdac" />
<img width="1854" height="1168" alt="Pasted image (5)" src="https://github.com/user-attachments/assets/1ce95284-21c9-4d95-aaf1-be96db0671b3" />

### 📍 **Login via SSO**
<img width="628" height="398" alt="Pasted image" src="https://github.com/user-attachments/assets/ac099f88-81a2-4ae4-863e-41b31a62c7bf" />
<img width="628" height="398" alt="Pasted image (2)" src="https://github.com/user-attachments/assets/2e3547f6-ab74-461d-9f8b-772cf07a915e" />
<img width="628" height="398" alt="Pasted image (3)" src="https://github.com/user-attachments/assets/c5d88959-9ed4-41aa-ad1d-b9cdc377fdaf" />

---

## ✅ Final Outcome

* Role-based authorization successfully implemented
* Job visibility restricted based on user roles
* Separate views created for each team
* Google SSO enabled for admin
* Jenkins secured with proper authentication & authorization

---

## 🧠 Key Learnings

* Difference between Jenkins authorization strategies
* Practical implementation of Role-Based Authorization
* User and role management in Jenkins
* Securing Jenkins using Google SSO

---

## 📌 Conclusion

This assignment demonstrates a **real-world Jenkins security setup**, ensuring:

* Controlled access
* Secure authentication
* Clear separation of responsibilities
