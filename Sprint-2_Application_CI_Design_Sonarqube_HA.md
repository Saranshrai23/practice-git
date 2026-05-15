# SonarQube High Availability (HA) — Documentation

---

## Author Information

| Author      | Created on | Version | Last updated by | Last edited on | L0 Reviewer | L1 Reviewer     | L2 Reviewer     |
| ----------- | ---------- | ------- | --------------- | -------------- | ----------- | --------------- | --------------- |
| Saransh Rai | 15-05-2026 | v1.0    | Saransh Rai     | 15-05-2026     | Anuj Jain   | Prashant Sharma | Piyush Upadhyay |

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [What is SonarQube HA](#2-what-is-sonarqube-ha)
3. [Why SonarQube HA is Needed](#3-why-sonarqube-ha-is-needed)
4. [SonarQube HA Architecture](#4-sonarqube-ha-architecture)
5. [SonarQube HA Workflow Diagram](#5-sonarqube-ha-workflow-diagram)
6. [How SonarQube HA is Done](#6-how-sonarqube-ha-is-done)
7. [Advantages of SonarQube HA](#7-advantages-of-sonarqube-ha)
8. [Best Practices](#8-best-practices)
9. [Conclusion](#9-conclusion)
10. [Contact Information](#10-contact-information)
11. [References](#11-references)

---

## 1. Introduction

This document explains how SonarQube High Availability (HA) is implemented to ensure the service remains available without downtime.

SonarQube is a tool used to check code quality, bugs, and security issues in applications. High Availability (HA) means the system remains available and does not stop working even if one server fails. In HA setup, multiple SonarQube servers are used so that the service continues without downtime.

*Note:* High Availability feature is available only in Enterprise and Data Center editions of SonarQube.

---

## 2. What is SonarQube HA

SonarQube High Availability is a setup where multiple SonarQube application servers run together.

If one server fails, another server handles the requests, and the service continues.

Main components used in HA:

* Multiple SonarQube servers
* Load Balancer
* Shared Database (PostgreSQL)
* Elasticsearch Cluster

---

## 3. Why SonarQube HA is Needed

* Ensures SonarQube remains available even if one server fails
* Prevents downtime and keeps code analysis accessible
* Ensures CI/CD pipelines continue working without interruption
* Distributes load for better performance

---

## 4. SonarQube HA Architecture

| Component                    | What it does                            |
| ---------------------------- | --------------------------------------- |
| Load Balancer                | Sends requests to working servers       |
| SonarQube Servers            | Show UI and API, run code checks        |
| Compute Engine               | Runs background tasks                   |
| Shared Database (PostgreSQL) | Stores projects, issues, and metrics    |
| Elasticsearch Cluster        | Helps search and indexing for all nodes |

*Notes:*

* All servers use the same database and Elasticsearch
* Background tasks are shared
* If one server fails, Load Balancer sends traffic to others

---

## 5. SonarQube HA Workflow Diagram

<img width="1400" height="1000" alt="mermaid-diagram" src="https://github.com/user-attachments/assets/8a776827-af5e-4d18-810f-5f3ad77868b5" />

---

*Workflow steps:*

* Developer pushes code
* CI/CD pipeline triggers SonarQube analysis
* Request goes to Load Balancer
* Load Balancer sends it to a working server
* Server runs analysis using Compute Engine
* Results go to PostgreSQL and Elasticsearch
* Users see results on the dashboard
* If a server fails, traffic goes to another server

---

## 6. How SonarQube HA is Done

1. *Setup Shared Database*

   * Install PostgreSQL on a separate server
   * Create SonarQube database
   * Connect all SonarQube servers to this database

2. *Setup Elasticsearch Cluster*

   * Install Elasticsearch on separate servers
   * Connect all SonarQube servers to Elasticsearch

3. *Install Multiple SonarQube Servers*

   * Install SonarQube on 2 or more servers
   * Use same database and Elasticsearch
   * Start all SonarQube servers

4. *Setup Load Balancer*

   * Add all servers to Load Balancer
   * Enable health checks to use only working servers

5. *Check HA Setup*

   * Open SonarQube via Load Balancer URL
   * Stop one server
   * Service should still work

---

## 7. Advantages of SonarQube HA

| Advantage         | Why it is useful                                 |
| ----------------- | ------------------------------------------------ |
| No Downtime       | CI/CD pipelines are not stopped                  |
| Reliability       | System is stable and safe                        |
| Scalability       | Add more servers easily                          |
| Performance       | Work is shared across servers                    |
| Enterprise Ready  | Supports big companies                           |
| High Availability | SonarQube keeps working even if one server fails |

---

## 8. Best Practices

| Best Practice                         | Description                                                           |
| ------------------------------------- | --------------------------------------------------------------------- |
| Use Enterprise or Data Center Edition | HA features are supported only in Enterprise and Data Center editions |
| Use Separate Elasticsearch Servers    | Improves performance and avoids resource contention                   |
| Use External PostgreSQL Database      | Ensures centralized and reliable data storage                         |
| Configure Load Balancer               | Automatically redirects traffic to healthy SonarQube nodes            |
| Enable Health Checks                  | Detects failed nodes and maintains availability                       |
| Take Regular Backups                  | Protects projects, configurations, and analysis data                  |
| Monitor System Resources              | Helps maintain stable HA performance                                  |
| Use Multiple SonarQube Nodes          | Prevents single point of failure                                      |

---

## 9. Conclusion

SonarQube HA keeps SonarQube running all the time. Multiple servers, Load Balancer, shared database, and Elasticsearch make it reliable and fast. This setup is needed for production and enterprise use.

---

## 10. Contact Information

| Contact Type | Details                                                                         |
| ------------ | ------------------------------------------------------------------------------- |
| Email        | [saransh.rai.snaatak@mygurukulam.co](mailto:saransh.rai.snaatak@mygurukulam.co) |

---

## 11. References

| Topic                                                                                                           | Description                                                                |
| --------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| [SonarQube Documentation](https://docs.sonarsource.com/sonarqube/)                                              | Official documentation for installation, configuration, and best practices |
| [SonarQube Architecture Overview](https://docs.sonarsource.com/sonarqube/latest/architecture/)                  | Explains SonarQube components and how they work                            |
| [PostgreSQL Documentation](https://www.postgresql.org/docs/)                                                    | Official PostgreSQL documentation used for shared database setup           |
| [Elasticsearch Documentation](https://www.elastic.co/guide/index.html)                                          | Official Elasticsearch documentation for clustering and indexing           |
| [NGINX Load Balancer Documentation](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/) | Explains load balancing concepts and configuration                         |

-----------|-------------|
| [https://docs.sonarsource.com/sonarqube/](https://docs.sonarsource.com/sonarqube/) | Official documentation for installation, configuration, and best practices |
| [https://docs.sonarsource.com/sonarqube/latest/architecture/](https://docs.sonarsource.com/sonarqube/latest/architecture/) | Explains SonarQube components and how they work |

---
