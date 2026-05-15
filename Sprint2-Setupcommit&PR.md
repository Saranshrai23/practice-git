# Setup Commit & PR (Pull Request) Workflow

---

| Author      | Created on | Version | Last updated by | Last edited on | L0 Reviewer | L1 Reviewer     | L2 Reviewer     |
| ----------- | ---------- | ------- | --------------- | -------------- | ----------- | --------------- | --------------- |
| Saransh Rai | 15-05-2026 | v1.0    | Saransh Rai     | 15-05-2026     | Anuj Jain   | Prashant Sharma | Piyush Upadhyay |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [Purpose](#2-purpose)
3. [Key Objectives](#3-key-objectives)
4. [Prerequisites](#4-prerequisites)
5. [Workflow Diagram](#5-workflow-diagram)
6. [Setting up a PR](#6-setting-up-a-pr)

   * 6.1 [Step-by-step guide to create a commit](#61-step-by-step-guide-to-create-a-commit)
   * 6.2 [Steps to create a PR](#62-steps-to-create-a-pr)
   * 6.3 [Merge a PR](#63-merge-a-pr)
7. [Conclusion](#7-conclusion)
8. [FAQ](#8-faq)
9. [Contact Information](#9-contact-information)
10. [References](#10-references)

* 3.1 [Step-by-step guide to create a commit](#31-step-by-step-guide-to-create-a-commit)
* 3.2 [Steps to create a PR](#32-steps-to-create-a-pr)
* 3.3 [Merge a PR](#33-merge-a-pr)

4. [Conclusion](#4-conclusion)
5. [FAQ](#5-faq)
6. [Contact Information](#6-contact-information)
7. [References](#7-references)

---

# 1. Introduction

A Pull Request (PR) is a request to merge changes from one branch into another branch such as main. It allows developers and reviewers to validate changes before merging code into the main repository.

This document explains the complete Commit and Pull Request workflow used in the project.

---

# 2. Purpose

The purpose of this document is to define a clear and standard Commit and Pull Request workflow so that code changes can be reviewed, validated, and merged into the main branch in a controlled way.

---

# 3. Key Objectives

| Objective             | Description                                       |
| --------------------- | ------------------------------------------------- |
| Standardized Workflow | Maintain a proper Git workflow for all developers |
| Code Review Process   | Ensure changes are reviewed before merge          |
| CI/CD Validation      | Validate builds and checks using Jenkins          |
| Branch Protection     | Prevent direct changes to main branch             |
| Better Collaboration  | Improve team coordination and code tracking       |
| Stable Repository     | Maintain clean and stable main branch             |

---

# 4. Prerequisites

Before following this workflow, the below prerequisites should be available:

| Prerequisite                   | Justification                                                                       |
| ------------------------------ | ----------------------------------------------------------------------------------- |
| Git Installed                  | Required to run Git commands such as clone, branch, add, commit, push, and pull     |
| Repository Access              | Required to access the project repository and push changes to the remote branch     |
| GitHub/GitLab Account          | Required to create and manage Pull Requests or Merge Requests                       |
| Feature Branch Naming Standard | Required to create branches in the expected format such as `feature-XXX`            |
| Jenkins/CI Pipeline Access     | Required to verify automated build, test, and validation checks before merge        |
| Reviewer Availability          | Required because at least two reviewers should review and approve the PR            |
| Lead/Maintainer Access         | Required because only an authorized person should merge the PR into the main branch |

---

# 5. Workflow Diagram

The following workflow demonstrates the complete Commit and Pull Request lifecycle.

<details>
<summary>Click to expand workflow diagram</summary>

![Workflow Diagram](https://github.com/user-attachments/assets/e4585d10-3ee8-4703-9ebb-504aec36a69d)

</details>

---

# 6. Setting up a PR

##     6.1 Step-by-step guide to create a commit

Follow the below steps to create commits and prepare a Pull Request.

###     Step 1 — Pull the latest code from main branch

```bash
git checkout main
git pull origin main
```

<details>
<summary>Click to expand screenshot</summary>

![Step 1](https://github.com/user-attachments/assets/cd51d69e-8cd4-4650-ac3f-f74a86e41bab)

</details>

###     Step 2 — Create and switch to a feature branch

```bash
# Create new feature branch
git checkout -b feature-XXX

# Switch to existing feature branch
# git checkout feature-XXX
```

<details>
<summary>Click to expand screenshot</summary>

![Step 2](https://github.com/user-attachments/assets/236119b9-eef3-4960-8820-54911b072733)

</details>

###     Step 3 — Make the required code changes

Modify files, add features, fix bugs, or update configurations as required.

###     Step 4 — Stage the changes

```bash
# Stage all files
git add .

# Stage specific files
# git add file-name
```

<details>
<summary>Click to expand screenshot</summary>

![Step 4](https://github.com/user-attachments/assets/9f26157c-355a-4e6e-88df-c4c5b84bb522)

</details>

###     Step 5 — Commit the changes

```bash
git commit -m "Added feature implementation"
```

<details>
<summary>Click to expand screenshot</summary>

![Step 5](https://github.com/user-attachments/assets/76f924b4-8414-4881-9b68-241798bdb3c7)

</details>

###     Step 6 — Push the feature branch to remote repository

```bash
# Push branch and set upstream
git push -u origin feature-XXX
```

<details>
<summary>Click to expand screenshot</summary>

![Step 6](https://github.com/user-attachments/assets/d018b479-c174-4d13-ae67-7175006f65b2)

</details>

###     Step 7 — Sync feature branch with latest main branch

```bash
# Fetch latest updates
git fetch origin
```

This step ensures the feature branch is updated before creating the Pull Request.

###     Step 8 — Open a Pull Request

Create a Pull Request in GitHub/GitLab using:

* Source Branch → feature-XXX
* Target Branch → main

---

##     6.2 Steps to create a PR

### Step 1 — Ensure feature branch is updated

Make sure:

* All commits are pushed
* Branch is updated with latest main branch
* No merge conflicts exist

### Step 2 — Open Pull Request in GitHub/GitLab

Create a new Pull Request using:

* Source branch → feature-XXX
* Target branch → main

### Step 3 — Add PR details

Add:

* Summary of changes
* Purpose of implementation
* Testing steps
* Screenshots if required
* Related ticket details

<details>
<summary>Click to expand screenshots</summary>

![PR Screenshot 1](https://github.com/user-attachments/assets/8a6a130b-53c3-4185-a4be-d7a09c2c8e38)

![PR Screenshot 2](https://github.com/user-attachments/assets/57c51841-851e-4a0a-b428-8bf476f8bac5)

</details>

---

##     6.3 Merge a PR

### Step 1 — Review Approval

Ensure all required reviewers approve the Pull Request.

### Step 2 — Validate CI/CD checks

Verify all validations pass successfully:

| Validation      | Purpose                             |
| --------------- | ----------------------------------- |
| Jenkins Build   | Ensures project builds successfully |
| Lint Checks     | Validates coding standards          |
| Unit Tests      | Verifies application functionality  |
| Security Checks | Detects vulnerabilities             |

### Step 3 — Resolve Merge Conflicts

If merge conflicts occur:

* Pull latest main branch changes
* Resolve conflicts manually
* Commit resolved changes
* Push updated branch

### Step 4 — Merge the Pull Request

Use the Merge option available in GitHub/GitLab.

Possible merge methods:

* Merge Commit
* Squash Merge
* Rebase Merge

### Step 5 — Delete feature branch

Delete the feature branch after successful merge to keep the repository clean.

<details>
<summary>Click to expand merge screenshots</summary>

![Merge 1](https://github.com/user-attachments/assets/db5bd654-2f10-464c-90b7-d0c03c98f464)

![Merge 2](https://github.com/user-attachments/assets/b82a80cd-798b-4927-983c-5598625a91fc)

![Merge 3](https://github.com/user-attachments/assets/bb5fe80c-3f2a-48fa-9629-0a12ffade216)

![Merge 4](https://github.com/user-attachments/assets/0eb93f06-e7ee-471a-873f-55f5f02710b0)

![Merge 5](https://github.com/user-attachments/assets/6bcb6ab4-7fb9-4107-bab1-429de8c11401)

</details>

---

# 7. Conclusion

Following the Commit and Pull Request workflow helps maintain proper code review, CI/CD validation, collaboration, and controlled code merges into the main branch. This process ensures better code quality, repository stability, and traceable development history.

---

# 8. FAQ

## 1. Why should developers avoid committing directly to main?

Direct commits to main can introduce unstable or unreviewed code. Feature branches and Pull Requests provide controlled development and review processes.

## 2. What happens if merge conflicts occur?

Developers must pull the latest changes from main, resolve conflicts manually, and push the updated code again.

## 3. Who can merge a Pull Request?

Only authorized reviewers, maintainers, or leads should merge Pull Requests into the main branch.

## 4. What is the purpose of CI/CD checks?

CI/CD checks automatically validate code quality, build status, testing, and security before merging.

## 5. What is the difference between Commit and Pull Request?

A Commit saves changes locally in Git history, whereas a Pull Request requests review and merging of those commits into another branch.

---

# 9. Contact Information

| Name        | Email Address                                                                   |
| ----------- | ------------------------------------------------------------------------------- |
| Saransh Rai | [saransh.rai.snaatak@mygurukulam.co](mailto:saransh.rai.snaatak@mygurukulam.co) |

---

# 10. References

| Link                                                                                                                           | Description                          |
| ------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------ |
| [https://docs.github.com/en/get-started/quickstart/github-flow](https://docs.github.com/en/get-started/quickstart/github-flow) | GitHub branch workflow documentation |
| [https://docs.github.com/en/pull-requests](https://docs.github.com/en/pull-requests)                                           | GitHub Pull Request documentation    |
| [https://docs.gitlab.com/ee/user/project/merge_requests/](https://docs.gitlab.com/ee/user/project/merge_requests/)             | GitLab Merge Request documentation   |
| [https://git-scm.com/doc](https://git-scm.com/doc)                                                                             | Official Git documentation           |
