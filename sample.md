<img width="200" height="200" alt="image" src="https://github.com/user-attachments/assets/18c1bea7-22ee-4e7c-97ae-3ce9a3bbb496" />

# **Setup Commit & PR (Pull Request) Workflow**

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
   * [4.1 Log in to GitHub](#41-log-in-to-github)
   * [4.2 Create a Repository](#42-create-a-repository)
   * [4.3 Create a Feature Branch](#43-create-a-feature-branch)
   * [4.4 Make a Change in the Feature Branch](#44-make-a-change-in-the-feature-branch)
   * [4.5 Create a Pull Request (PR)](#45-create-a-pull-request-pr)
   * [4.6 Navigate to Branch Rules Configuration](#46-navigate-to-branch-rules-configuration)
   * [4.7 Enforce Reviewer Sign-Off Rules](#47-enforce-reviewer-sign-off-rules)
   * [4.8 Enforce Jenkins System Validation](#48-enforce-jenkins-system-validation)
   * [4.9 Restrict Merge Access to Lead](#49-restrict-merge-access-to-lead)
5. [Conclusion](#5-conclusion)
6. [Contact Information](#6-contact-information)
7. [Reference Table](#7-reference-table)

---

# **1. Introduction**

This document explains how to configure a secure **Commit and Pull Request (PR) workflow** using GitHub branch protection rules and Jenkins validation. The workflow ensures proper code review, automated testing, and controlled merge access before code reaches the `main` branch.

---

# **2. Prerequisites**

| Prerequisite | Description |
|---|---|
| GitHub Repository Access | GitHub repository with admin access to configure branch rules and PR settings |
| Jenkins Server | Jenkins server configured for CI pipeline validation and PR build execution |
| GitHub Webhook Integration | GitHub webhook integrated with Jenkins for automatic PR and push event triggering |
| Repository Users & Reviewers | Users and reviewers added to the repository/team for approval and collaboration workflow |

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

## <a name="41-log-in-to-github"></a>&nbsp;&nbsp;&nbsp;&nbsp;4.1 Log in to GitHub

Visit `https://github.com` and sign in using your GitHub credentials.

<img width="1907" height="911" alt="image" src="https://github.com/user-attachments/assets/113482c0-b12a-47ad-8fca-899dced88f0e" />


---

## <a name="42-create-a-repository"></a>&nbsp;&nbsp;&nbsp;&nbsp;4.2 Create a Repository

Create a new GitHub repository where the PR workflow configuration will be implemented.

<img width="1917" height="960" alt="image" src="https://github.com/user-attachments/assets/4c19e5e8-281d-4493-9577-1ac1e4e838c5" />


---

## <a name="43-create-a-feature-branch"></a>&nbsp;&nbsp;&nbsp;&nbsp;4.3 Create a Feature Branch

Create a new branch named `feature-001` from the `main` branch. 

<img width="1912" height="942" alt="image" src="https://github.com/user-attachments/assets/f4474c91-57a9-4431-ba7c-f090fdefefc3" />



---

## <a name="44-make-a-change-in-the-feature-branch"></a>&nbsp;&nbsp;&nbsp;&nbsp;4.4 Make a Change in the Feature Branch

Switch to the feature-001 branch and make a small change—for example, add a new line of code. Commit this change to simulate a code update.

<img width="1917" height="967" alt="image" src="https://github.com/user-attachments/assets/9a34e6b8-070f-48e9-a0fc-ebca1715e289" />



---

## <a name="45-create-a-pull-request-pr"></a>&nbsp;&nbsp;&nbsp;&nbsp;4.5 Create a Pull Request (PR)

After committing the change, open a pull request from feature-001 to the main branch. This PR can currently be merged unless rules are set.

<img width="1916" height="960" alt="image" src="https://github.com/user-attachments/assets/b544b64d-b80c-4592-b3d4-1f451182b8d5" />


---

## <a name="46-navigate-to-branch-rules-configuration"></a>&nbsp;&nbsp;&nbsp;&nbsp;4.6 Navigate to Branch Rules Configuration

To enforce proper PR rules, Go to:

`Repository → Settings → Branches → Add Rule`

Configure branch protection rules for the `main` branch.

<img width="1917" height="971" alt="image" src="https://github.com/user-attachments/assets/9403b7f5-0098-4cb3-aaa6-373466baafd9" />


---

## <a name="47-enforce-reviewer-sign-off-rules"></a>&nbsp;&nbsp;&nbsp;&nbsp;4.7 Enforce Reviewer Sign-Off Rules

Enable:

* **Require a pull request before merging**
* Set **Required approvals = 2**

This ensures at least two reviewers approve the PR before merge.

<img width="1917" height="895" alt="image" src="https://github.com/user-attachments/assets/c372d407-441a-4f17-a578-375f306e4ebb" />


---

## <a name="48-enforce-jenkins-system-validation"></a>&nbsp;&nbsp;&nbsp;&nbsp;4.8 Enforce Jenkins System Validation

Enable:

* **Require status checks to pass before merging**

This ensures Jenkins validates build, test, and linting pipelines before merge approval.

<img width="1912" height="756" alt="image" src="https://github.com/user-attachments/assets/0f378309-7547-40da-afeb-d8680f06c49f" />


---

## <a name="49-restrict-merge-access-to-lead"></a>&nbsp;&nbsp;&nbsp;&nbsp;4.9 Restrict Merge Access to Lead

Enable:

* **Restrict who can push to matching branches**

Add Team Lead users so only authorized leads can merge PRs into `main`.

<img width="982" height="492" alt="image" src="https://github.com/user-attachments/assets/7861d7a8-bbde-4b77-a978-917cd5d693c8" />


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
