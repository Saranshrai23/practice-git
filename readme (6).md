---

# 📘 Assignment 02

## Deployment Strategies with Amazon S3 Integration

---

## 🎯 Objective

The objective of this assignment is to **study, compare, and implement various deployment strategies** while integrating **Amazon S3** for managing static assets and deployment artifacts.

This assignment covers both **theoretical understanding** and **practical implementation** using AWS services.

---

## 🧠 Deployment Strategies Covered

### Must Do

1. Recreate Deployment
2. Rolling Deployment

### Good To Do

3. Blue-Green Deployment
4. Canary Deployment

### Conceptual

5. A/B Deployment

---

# 🔹 PART 1: Research & Understanding of Deployment Strategies

---

## 1️⃣ Recreate Deployment

### 📌 Description

In Recreate Deployment, the **existing application instances are completely terminated**, and a **new version of the application is deployed on fresh instances**.

### ✅ Advantages

* Simple to implement
* No version conflicts
* Clean and immutable deployment

### ❌ Disadvantages

* Application downtime occurs
* Not suitable for high-availability systems

---

## 2️⃣ Rolling Deployment

### 📌 Description

In Rolling Deployment, the application is updated **gradually**, where **old instances are replaced one by one** with new instances.

### ✅ Advantages

* No downtime
* Safer than recreate deployment
* Suitable for production workloads

### ❌ Disadvantages

* Old and new versions coexist temporarily
* Rollback can be complex

---

## 3️⃣ Blue-Green Deployment

### 📌 Description

Blue-Green Deployment uses **two identical environments**:

* Blue → current live environment
* Green → new version environment

Traffic is switched from Blue to Green using a Load Balancer.

### ✅ Advantages

* Zero downtime
* Easy rollback
* Full testing before traffic switch

### ❌ Disadvantages

* Higher cost (duplicate infrastructure)

---

## 4️⃣ Canary Deployment

### 📌 Description

In Canary Deployment, the new version is released to a **small subset of users/instances**, while the majority still use the old version.

###  Advantages

* Very low risk
* Easy monitoring and validation
* Gradual rollout

###  Disadvantages

* Requires advanced monitoring
* More complex setup

---

## 5️⃣ A/B Deployment (Conceptual)

### 📌 Description

A/B Deployment delivers **two different application versions** to different user groups to compare behavior and performance.

###  Advantages

* Helps in feature testing
* Data-driven decisions

###  Disadvantages

* Complex traffic routing
* Requires analytics integration

---

# 🔹 PART 2: Implementation

---

## ✅ MUST DO – Implementation

---

# 🚀 Recreate Deployment (Implemented)

###  Architecture Overview

```
User → Browser → EC2 (Nginx) → Static Assets from S3
                 ↑
              Custom AMI
```

---

###  Step-by-Step Implementation

#### Step 1: Launch Base EC2 Instance

* Launch an EC2 instance (Ubuntu)
* Install Nginx
* Deploy a basic application

📸 **Screenshot :**
<img width="1919" height="972" alt="image" src="https://github.com/user-attachments/assets/40737fcb-03b0-42df-ac01-cb8d37d44054" />


---

#### Step 2: Store Static Assets in S3

* Create an S3 bucket
* Upload static assets:

  * HTML
  * CSS
  * Images
* Make objects public or accessible via IAM role

📸 **Screenshot :**
<img width="1919" height="833" alt="image" src="https://github.com/user-attachments/assets/dba63b0d-13df-44bf-aa02-3ed164f6e8b6" />


---

#### Step 3: Create Custom AMI

* Create an AMI snapshot from the EC2 instance
* This AMI will be used for recreate deployment

📸 **Screenshot :**
<img width="1919" height="834" alt="image" src="https://github.com/user-attachments/assets/db68380e-93b3-45fb-9aca-c2600db9ec20" />


---

#### Step 4: Launch New Instance Using AMI

* Launch a new EC2 instance using the custom AMI
* Use **User Data** to fetch updated assets from S3

```bash
#!/bin/bash
apt update -y

cd /tmp

# Download file and rename as index.html
wget -O index.html https://nginx-static-website-demo-123.s3.ap-northeast-3.amazonaws.com/recreate.html

# Replace version
sed -i 's/v1.0/v2.0/g' index.html

# Replace nginx default page
rm -f /var/www/html/index.html
cp index.html /var/www/html/index.html

# Restart nginx
systemctl restart nginx

```

---

#### Step 5: Terminate Old Instance

* Old instance is terminated
* New instance serves the updated application

📸 **Screenshot :**
<img width="1919" height="967" alt="image" src="https://github.com/user-attachments/assets/fb5d7a5a-33df-431d-884e-903622311b4b" />

---

### ✅ Result

* Application version updated
* New EC2 instance created
* Old instance removed (Recreate Deployment)

---

# 🚀 Rolling Deployment (Implemented)

---

###  Architecture Overview

```
User → Load Balancer → Auto Scaling Group → EC2 Instances
                               ↑
                     Launch Configuration (v1 → v2)
```

---

###  Step-by-Step Implementation

#### Step 1: Create launch template version 1 and Auto Scaling Group  

* launch template using an ubuntu image and using below user data

```
#!/bin/bash
apt update -y
apt install nginx -y

cd /tmp
wget -O index.html https://priyanshu-rolling-bucket.s3.ap-northeast-3.amazonaws.com/Rolling-index-v1.html

rm -f /var/www/html/index.html
cp index.html /var/www/html/index.html

systemctl restart nginx

```

* Minimum instances: 2
* Maximum instances: 4
* Attach to Application Load Balancer

📸 **Screenshot :**
<img width="1689" height="749" alt="image" src="https://github.com/user-attachments/assets/277af0e5-8237-4fc9-9ae8-b46219337102" />


---

#### Step 2: Store Deployment Artifacts in S3

* Upload application files to S3
* ASG instances pull artifacts during launch

📸 **Screenshot :**
<img width="1919" height="788" alt="image" src="https://github.com/user-attachments/assets/4331d4c6-361c-4d26-b00f-ae011284a5b9" />

---

#### Step 3: Deploy Initial Version

* Launch ASG with version v1.0
* Verify application through Load Balancer

📸 **Screenshot Placeholder:**
<img width="1919" height="968" alt="image" src="https://github.com/user-attachments/assets/6852d6dd-abd7-4f2a-9521-25882eaee400" />


---

#### Step 4: Create New Launch Configuration

* Update application version to v2.0
* Modify user data to fetch new artifacts from S3
* Version 2 User data:
```
#!/bin/bash
apt update -y
apt install nginx -y

cd /tmp
wget -O index.html https://priyanshu-rolling-bucket.s3.ap-northeast-3.amazonaws.com/Rolling-index-v2.html

rm -f /var/www/html/index.html
cp index.html /var/www/html/index.html

systemctl restart nginx
```

📸 **Screenshot :**
<img width="1919" height="835" alt="image" src="https://github.com/user-attachments/assets/b2e89673-18b2-441e-a983-9ff729b7b375" />


---

#### Step 5: Update ASG

* Update ASG to use the new launch configuration
* Instances are replaced gradually

📸 **Screenshot :**
<img width="1905" height="769" alt="image" src="https://github.com/user-attachments/assets/9d6a809f-e044-475d-896a-3ab1d47af604" />

Now Do instance refresh and you will see new template is launching new instance and it is updating the app to version 2. Also on some instances the old version is still running.After sometime all the instances will be having hte new version.
<img width="1919" height="960" alt="image" src="https://github.com/user-attachments/assets/a94e794a-bdd3-47ca-acd2-6cf42bf04e45" />


---

### ✅ Result

* Zero downtime deployment
* Gradual replacement of instances
* Rolling deployment successfully implemented

---

## 🔶 GOOD TO DO – Implementation

---

# 🔵🟢 Blue-Green Deployment

### Implementation Summary

* Two identical EC2 environments (Blue & Green)
* Load Balancer switches traffic
* Configuration files stored in S3

📸 **Screenshot Placeholders:**

* `[Screenshot: Blue environment running]`
* `[Screenshot: Green environment running]`
* `[Screenshot: Load Balancer target group switch]`

---

# 🐤 Canary Deployment

### Implementation Summary

* New version deployed to a small subset of instances
* Metrics/logs stored in S3
* Performance monitored before full rollout

📸 **Screenshot Placeholders:**

* `[Screenshot: Canary instances deployed]`
* `[Screenshot: Logs stored in S3]`

---

# 📊 Comparison Table

| Strategy   | Downtime | Risk     | Cost   | Complexity |
| ---------- | -------- | -------- | ------ | ---------- |
| Recreate   | Yes      | High     | Low    | Low        |
| Rolling    | No       | Medium   | Medium | Medium     |
| Blue-Green | No       | Low      | High   | High       |
| Canary     | No       | Very Low | Medium | High       |
| A/B        | No       | Medium   | High   | Very High  |

---

# ✅ Conclusion

This assignment demonstrates:

* Practical implementation of deployment strategies
* Effective use of Amazon S3 for asset management
* Understanding of production-grade deployment patterns
* Hands-on experience with EC2, ASG, AMI, and Load Balancer

---

