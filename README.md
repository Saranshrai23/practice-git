# 🚀 Assignment 2  
## 🌐 Deployment Strategies with Amazon S3 Integration  

<p align="center">
  <img src="https://img.shields.io/badge/AWS-EC2-orange?style=for-the-badge&logo=amazonaws">
  <img src="https://img.shields.io/badge/DevOps-Deployment-blue?style=for-the-badge&logo=devops">
  <img src="https://img.shields.io/badge/Nginx-Web%20Server-green?style=for-the-badge&logo=nginx">
  <img src="https://img.shields.io/badge/S3-Static%20Assets-yellow?style=for-the-badge&logo=amazons3">
</p>

---

## 🎯 Objective

🎯 The purpose of this assignment is to **explore, compare, and implement modern deployment strategies** using AWS services while integrating **Amazon S3** for:

- 📦 Static asset management  
- 🧩 Deployment artifacts  
- 🔁 Version-controlled rollouts  

This project combines **theoretical understanding** 📘 with **hands-on AWS implementation** 🛠️, reflecting real-world DevOps workflows.

---

## 🧠 Deployment Strategies Covered

### ✅ Core Implementations
- 🔴 **Recreate Deployment**
- 🔄 **Rolling Deployment**

### ⭐ Additional Strategies
- 🔵🟢 **Blue-Green Deployment**
- 🐤 **Canary Deployment**

### 💡 Conceptual Coverage
- 🧪 **A/B Deployment**

---

# 📘 PART 1: Deployment Strategy Concepts

---

## 🔴 1. Recreate Deployment

### 📌 Overview
In **Recreate Deployment**, all existing application instances are **terminated first**, and the **new version is deployed from scratch**.

### ✅ Advantages
✔ Simple to implement  
✔ No version conflicts  
✔ Clean & immutable infrastructure  

### ❌ Disadvantages
✖ Application downtime  
✖ Not suitable for HA systems  

---

## 🔄 2. Rolling Deployment

### 📌 Overview
In **Rolling Deployment**, instances are **updated gradually**, replacing old versions **one by one**.

### ✅ Advantages
✔ Zero downtime  
✔ Safer than recreate  
✔ Production-friendly  

### ❌ Disadvantages
✖ Mixed versions during rollout  
✖ Rollback can be complex  

---

## 🔵🟢 3. Blue-Green Deployment

### 📌 Overview
Two identical environments are maintained:

- 🔵 **Blue** → Live production  
- 🟢 **Green** → New version  

Traffic is switched using a **Load Balancer**.

### ✅ Advantages
✔ Zero downtime  
✔ Instant rollback  
✔ Full pre-release testing  

### ❌ Disadvantages
✖ Higher infrastructure cost  

---

## 🐤 4. Canary Deployment

### 📌 Overview
A new version is released to a **small subset of users or instances**, while others continue on the old version.

### ✅ Advantages
✔ Very low risk  
✔ Easy validation  
✔ Gradual rollout  

### ❌ Disadvantages
✖ Requires monitoring & metrics  
✖ Complex setup  

---

## 🧪 5. A/B Deployment (Conceptual)

### 📌 Overview
Two application versions are served to **different user groups** for comparison.

### ✅ Advantages
✔ Feature experimentation  
✔ Data-driven decisions  

### ❌ Disadvantages
✖ Complex routing  
✖ Analytics required  

---

# 🛠️ PART 2: Practical Implementation

---

## 🚀 Recreate Deployment (Implemented)

### 🏗️ Architecture

