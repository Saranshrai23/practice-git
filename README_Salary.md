# Salary API

---

| Author      | Created on | Version | Last updated by | Last Edited On | L0 Reviewer | L1 Reviewer     | L2 Reviewer     |
| ----------- | ---------- | ------- | --------------- | -------------- | ----------- | --------------- | --------------- |
| Saransh Rai | 19-04-2026 | 1.0     | Saransh Rai     | 19-04-2026     | Anuj Jain   | Prashant Sharma | Piyush Upadhyay |

---

## Table of Contents

1. [Introduction](#introduction)
2. [Purpose](#purpose)
3. [Pre-Requisites](#pre-requisites)
4. [System Requirements](#system-requirements)
5. [Dependencies](#dependencies)
6. [Important Ports](#important-ports)
7. [Architecture](#architecture)
8. [Dataflow Diagram](#dataflow-diagram)
9. [Monitoring](#monitoring)
10. [Health Check](#health-check)
11. [Logging](#logging)
12. [Disaster Recovery](#disaster-recovery)
13. [High Availability](#high-availability)
14. [Troubleshooting](#troubleshooting)
15. [FAQs](#faqs)
16. [How to Bring Up the Salary API](#-how-to-bring-up-the-salary-api)
17. [Contact Information](#12-contact-information)
18. [References](#references)

---

## Introduction

The *Salary API* is a **Spring Boot-based microservice** designed to handle salary-related operations in a scalable and modular way within the OT-Microservices ecosystem. It follows a **microservices architecture**, allowing independent deployment, better fault isolation, and seamless integration with other services like employee and attendance APIs. The system uses **ScyllaDB for high-performance distributed storage** and **Redis for caching**, ensuring fast response times and efficient data handling. This API is built to support modern DevOps practices such as containerization, CI/CD pipelines, and cloud-native deployments.

---

## Purpose

The *Salary API* is a Java-based microservice developed as part of the *OT-Microservices stack*.
The primary purpose of this application is to manage *salary-related transactions and records* in a scalable, secure, and independent manner.

### Why this application is needed

* To separate salary management from other HR functionalities
* To enable independent deployment and scaling of salary services
* To handle financial data with better performance and reliability
* To support high-volume salary queries using a distributed database

This application addresses challenges such as:

* Tight coupling in monolithic HR systems
* Limited scalability of legacy applications
* Lack of observability and standardized deployment processes

---

## Pre-Requisites

Before deploying the Salary API, ensure that the following *hardware, software, and security requirements* are met.

---

## System Requirements

### Hardware Specifications

| Hardware  | Minimum Recommendation |
| --------- | ---------------------- |
| Processor | Dual-core              |
| RAM       | 4 GB                   |
| Disk      | 20 GB                  |
| OS        | Ubuntu 22.04           |

---

## Dependencies

### Build Time Dependencies

| Name       | Version     | Description                         |
| ---------- | ----------- | ----------------------------------- |
| Java (JDK) | 11 or above | Required to compile the application |
| Maven      | 3.x         | Build and dependency management     |

---

### Run Time Dependencies

| Name               | Version              | Description                      |
| ------------------ | -------------------- | -------------------------------- |
| Java Runtime (JRE) | 11 or above          | Required to run the application  |
| ScyllaDB           | Cassandra compatible | Primary database for salary data |
| Redis              | Latest stable        | Cache management                 |
| Migrate            | Latest               | Database schema migration        |

---

### Other Dependencies

| Name       | Version  | Description                   |
| ---------- | -------- | ----------------------------- |
| Prometheus | Optional | Metrics scraping              |
| Swagger UI | Bundled  | API documentation and testing |

---

## Important Ports

### Inbound Traffic

| Port | Description      |
| ---- | ---------------- |
| 9042 | Used by ScyllaDB |

### Outbound Traffic

| Port | Description                    |
| ---- | ------------------------------ |
| 8080 | Used by embedded Tomcat server |

---

## Architecture

The Salary API follows a *microservices-based architecture*, where the service runs independently and communicates over HTTP.

*Key components:*

* Client (Frontend or API consumer)
* Salary API (Spring Boot application)
* Redis (Cache layer)
* ScyllaDB (Persistent data store)
* Monitoring systems

<img width="1759" height="894" alt="image" src="https://github.com/user-attachments/assets/c81b1a3a-70cf-4e51-a1eb-08d45d16be23" />


---

## Dataflow Diagram

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/dec9b1df-4dbc-400e-aa5c-87047b7db2be" />


### Data Flow Explanation

1. Client sends an HTTP request to the Salary API
2. Salary API checks Redis for cached data
3. If cache hit, the response is returned immediately
4. If cache miss, data is fetched from ScyllaDB
5. Retrieved data is cached in Redis
6. Response is returned to the client
7. Metrics are exposed for monitoring

---

## Run Time Dependencies

The following runtime dependencies must be installed and configured before running the Salary API:

| Name     | Version              | Description                                           |
| -------- | -------------------- | ----------------------------------------------------- |
| ScyllaDB | Cassandra compatible | Primary database used to store salary-related data    |
| Redis    | Latest stable        | Cache manager for storing frequently accessed data    |
| Migrate  | Latest               | Database migration tool used to manage schema changes |

> *Note:* Installation steps may vary based on the target environment, such as cloud or on-premises deployment.

---

## Other Dependencies

The following dependencies are optional and are mainly used for monitoring and API validation:

| Name       | Version  | Description                                                  |
| ---------- | -------- | ------------------------------------------------------------ |
| Prometheus | Optional | Used for collecting application metrics                      |
| Swagger UI | Bundled  | Provides interactive API documentation and testing interface |

---

## Monitoring

Monitoring ensures application health, performance, and availability.

## Metrics

| Parameter          | Description                     | Priority | Threshold      | Command to Check                                                                |            |
| ------------------ | ------------------------------- | -------- | -------------- | ------------------------------------------------------------------------------- | ---------- |
| Disk Utilization   | Disk space used by application  | High     | > 90%          | `df -h`                                                                         |            |
| Availability       | Application uptime              | High     | >= 99.9%       | `uptime`                                                                        |            |
| Memory Utilization | Memory usage                    | Medium   | > 80%          | `free -h`                                                                       |            |
| CPU Utilization    | CPU usage                       | Medium   | > 70%          | `top` or `htop`                                                                 |            |
| Network Traffic    | Network usage                   | Medium   | Varies         | `iftop` or `netstat -i`                                                         |            |
| Latency            | Response time                   | High     | < 300ms        | `curl -w "%{time_total}" -o /dev/null -s http://localhost:8082/actuator/health` |            |
| Errors             | Error rate                      | High     | > 5 per minute | `tail -f ~/salary.log`                                                          |            |
| Throughput         | Requests per minute             | High     | > 1000         | `netstat -an                                                                    | grep 8082` |
| Security           | Authentication & access control | High     | Continuous     | `grep "ERROR\|FAIL" ~/salary.log`                                               |            |

---

## Health Check

| Name       | Type           | InitialDelaySeconds | PeriodSeconds | TimeoutSeconds | SuccessThreshold | FailureThreshold |
| ---------- | -------------- | ------------------- | ------------- | -------------- | ---------------- | ---------------- |
| Salary API | ReadinessProbe | 10                  | 10            | 5              | 1                | 3                |
| Salary API | LivenessProbe  | 10                  | 10            | -              | 5                | 1                |

## Explanation of Health Parameters

| Parameter           | Description                                           |
| ------------------- | ----------------------------------------------------- |
| ReadinessProbe      | Checks if the application is ready to receive traffic |
| LivenessProbe       | Checks if the application is running                  |
| InitialDelaySeconds | Delay before the first health check is performed      |
| PeriodSeconds       | Frequency at which health checks are executed         |
| TimeoutSeconds      | Maximum time to wait for a health check response      |
| SuccessThreshold    | Number of consecutive successful checks required      |
| FailureThreshold    | Number of consecutive failed checks allowed           |

---

## Logging

| Log Type    | Location               | Description                           |
| ----------- | ---------------------- | ------------------------------------- |
| Event Logs  | location/to/event.log  | Application-level events              |
| Access Logs | location/to/access.log | Authentication and authorization logs |
| Server Logs | location/to/server.log | Server activity and runtime logs      |
| Threat Logs | location/to/threat.log | Security-related events and threats   |

---

## Disaster Recovery

Disaster Recovery (DR) ensures service restoration after unexpected events that impact application availability or data integrity.

### Disaster scenarios include:

* Hardware failures
* Network outages
* Security incidents
* Data corruption

### Recovery strategies include:

* Regular database backups
* Configuration and application backups
* Redeployment of the application on alternate infrastructure

---

## High Availability

High Availability (HA) focuses on minimizing application downtime and ensuring continuous service availability.

### High Availability is achieved by:

* Running multiple instances of the application
* Using load balancers to distribute traffic
* Ensuring database replication and redundancy

*Note:*
Disaster Recovery focuses on restoring services after a failure, whereas High Availability focuses on preventing downtime.

---

## Troubleshooting

| Issue                    | Resolution                                          |
| ------------------------ | --------------------------------------------------- |
| Application not starting | Verify Java version and application configuration   |
| Migration failure        | Validate credentials and settings in migration.json |
| Redis connection error   | Ensure Redis service is running and reachable       |
| Health endpoint DOWN     | Verify database connectivity and service status     |

---

## FAQs

*Is this application free?*
Yes, this is an open-source application.

*Can it be deployed on all cloud platforms?*
Yes, the application is cloud-agnostic and can be deployed on any cloud platform.

*Is an enterprise version available?*
No, only the open-source version is available.

---

## 🚀 How to Bring Up the Salary API

To set up and run this application, refer to the official repository:

👉 [Salary POC README Documentation](add_link_versha)

---

## 12. Contact Information

| Name        | Email                                                                           |
| ----------- | ------------------------------------------------------------------------------- |
| Saransh Rai | [saransh.rai.snaatak@mygurukulam.co](mailto:saransh.rai.snaatak@mygurukulam.co) |

---

## References

| Topic                                                                                                                       | Description                                                              |
| --------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| [Jenkins Installation Guide](https://www.jenkins.io/doc/book/installing/linux/#debianubuntu)                                | Official Jenkins documentation for installation on Linux systems         |
| [FAQ Structure Reference](https://amplifi.com/user-guide/FAQs.html)                                                         | Guide for structuring FAQ sections in documentation                      |
| [Introduction vs Overview](https://thecontentauthority.com/blog/introduction-vs-overview)                                   | Explanation of difference between introduction and overview sections     |
| [Salary API POC Repository](https://github.com/Snaatak-Error-404/Sprint-1/blob/SCRUM-76-ajitesh/OT_MS/Salary/POC/readme.md) | GitHub repository containing Salary API proof of concept and setup steps |
