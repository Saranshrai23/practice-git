# **Setup Commit & PR (Pull Request) Workflow**

<p align="center">
  <img src="https://cdn.creazilla.com/icons/3214459/git-pull-request-icon-size_512.png" alt="Pull Request Icon" width="200"/>
</p>

## **Author Information**

| Author      | Created on | Version | Last updated by | Last edited on | L0 Reviewer | L1 Reviewer     | L2 Reviewer     |
| ----------- | ---------- | ------- | --------------- | -------------- | ----------- | --------------- | --------------- |
| Saransh Rai | 12-05-2025 | v1.0    | Saransh Rai     | 20-05-2026     | Anuj Jain   | Prashant Sharma | Piyush Upadhyay |

---

# **Table of Contents**

1. [Introduction](#1-introduction)
2. [Prerequisites](#2-prerequisites)
3. [Commit & PR Workflow Strategy](#3-commit--pr-workflow-strategy)
4. [Step-by-Step Instructions](#4-step-by-step-instructions)
   4.1 [Log in to GitHub](#41-log-in-to-github)
   4.2 [Create a Repository](#42-create-a-repository)
   4.3 [Create a Feature Branch](#43-create-a-feature-branch)
   4.4 [Make a Change in the Feature Branch](#44-make-a-change-in-the-feature-branch)
   4.5 [Create a Pull Request (PR)](#45-create-a-pull-request-pr)
   4.6 [Navigate to Branch Rules Configuration](#46-navigate-to-branch-rules-configuration)
   4.7 [Enforce Reviewer Sign-Off Rules](#47-enforce-reviewer-sign-off-rules)
   4.8 [Enforce Jenkins System Validation](#48-enforce-jenkins-system-validation)
   4.9 [Restrict Merge Access to Lead](#49-restrict-merge-access-to-lead)
5. [Conclusion](#5-conclusion)
6. [Contact Information](#6-contact-information)
7. [Reference Table](#7-reference-table)

---

# **1. Introduction**

This document explains how to configure a secure **Commit and Pull Request (PR) workflow** using GitHub branch protection rules and Jenkins validation. The workflow ensures proper code review, automated testing, and controlled merge access before code reaches the `main` branch.

---

# **2. Prerequisites**

* GitHub repository with admin access
* Jenkins server configured for CI validation
* GitHub webhook integration with Jenkins
* Users and reviewers added to the repository/team

---

# **3. Commit & PR Workflow Strategy**

| Criteria               | Description                                           |
| ---------------------- | ----------------------------------------------------- |
| **Source Branch**      | Must follow naming format like `feature-001`          |
| **Target Branch**      | Pull Requests should target the `main` branch         |
| **Reviewer Sign-Offs** | Minimum 2 approvals required                          |
| **Jenkins Validation** | CI pipeline must pass before merge                    |
| **Merge Access**       | Only Team Lead can merge into `main`                  |
| **Code Validation**    | Automated linting, build, and testing through Jenkins |

---

# **4. Step-by-Step Instructions**

## <a name="41-log-in-to-github"></a>     4.1 Log in to GitHub

Visit `https://github.com` and sign in using your GitHub credentials.

![1](https://github.com/user-attachments/assets/0e0e6015-6b1a-4ac1-a5f9-89402751839e)

---

## <a name="42-create-a-repository"></a>     4.2 Create a Repository

Create a new GitHub repository where the PR workflow configuration will be implemented.

<img width="1917" height="960" alt="image" src="https://github.com/user-attachments/assets/4c19e5e8-281d-4493-9577-1ac1e4e838c5" />


---

## <a name="43-create-a-feature-branch"></a>     4.3 Create a Feature Branch

Create a new branch named `feature-001` from the `main` branch.

<img width="1917" height="941" alt="image" src="https://github.com/user-attachments/assets/55ddc5ae-1483-4c18-82b8-9b81b95d8045" />


---

## <a name="44-make-a-change-in-the-feature-branch"></a>     4.4 Make a Change in the Feature Branch

Switch to the feature branch, make a code change, and commit the update.

![4](https://github.com/user-attachments/assets/9f0ecca1-7bd7-4fda-860b-f612074a6707)

---

## <a name="45-create-a-pull-request-pr"></a>     4.5 Create a Pull Request (PR)

Open a Pull Request from `feature-001` to the `main` branch.

![5](https://github.com/user-attachments/assets/10cab73a-c1f6-4ff4-b233-c2398694b4cf)

---

## <a name="46-navigate-to-branch-rules-configuration"></a>     4.6 Navigate to Branch Rules Configuration

Go to:

`Repository → Settings → Branches → Add Rule`

Configure branch protection rules for the `main` branch.

![7](https://github.com/user-attachments/assets/59eeb11d-3a46-4be6-a478-5d465b6f09ef)

---

## <a name="47-enforce-reviewer-sign-off-rules"></a>     4.7 Enforce Reviewer Sign-Off Rules

Enable:

* **Require a pull request before merging**
* Set **Required approvals = 2**

This ensures at least two reviewers approve the PR before merge.

![image](https://github.com/user-attachments/assets/35b021c4-1b7a-4383-a39b-118e8ea21c4a)

---

## <a name="48-enforce-jenkins-system-validation"></a>     4.8 Enforce Jenkins System Validation

Enable:

* **Require status checks to pass before merging**

This ensures Jenkins validates build, test, and linting pipelines before merge approval.

![image](https://github.com/user-attachments/assets/0771221b-12f8-4ece-8759-7ae3a9f5c215)

---

## <a name="49-restrict-merge-access-to-lead"></a>     4.9 Restrict Merge Access to Lead

Enable:

* **Restrict who can push to matching branches**

Add Team Lead users so only authorized leads can merge PRs into `main`.

![image](https://github.com/user-attachments/assets/2b6757f2-f354-4206-aaa2-e160cf1d29ca)

---

# **5. Conclusion**

This workflow helps enforce secure and reliable code management practices using GitHub branch protection and Jenkins CI validation. Mandatory approvals, automated checks, and restricted merge access ensure only validated and reviewed code is merged into the production branch.

---

# **6. Contact Information**

| Name        | Email Address                                                                   |
| ----------- | ------------------------------------------------------------------------------- |
| Saransh Rai | [saransh.rai.snaatak@mygurukulam.co](mailto:saransh.rai.snaatak@mygurukulam.co) |

---

# **7. Reference Table**

| Title                             | Link                                                                                                                                                                                                                                             | Purpose                                |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------- |
| GitHub Pull Request Documentation | [https://docs.github.com/en/pull-requests](https://docs.github.com/en/pull-requests)                                                                                                                                                             | Understand Pull Request workflow       |
| Jenkins Pipeline Documentation    | [https://www.jenkins.io/doc/book/pipeline/](https://www.jenkins.io/doc/book/pipeline/)                                                                                                                                                           | Configure Jenkins CI pipelines         |
| GitHub Branch Protection Rules    | [https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches) | Configure secure branch rules          |
| Jenkins GitHub Plugin             | [https://plugins.jenkins.io/github/](https://plugins.jenkins.io/github/)                                                                                                                                                                         | Integrate Jenkins with GitHub webhooks |
