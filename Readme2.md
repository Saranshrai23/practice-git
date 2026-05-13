<p align="left">
  <img width="30%" height="1000" alt="image" src="https://github.com/user-attachments/assets/e768852c-a32a-4cee-987d-17afc7f96fca" />
  <br/>
</p>

<h1 align="left">OT MS Understanding | Salary | Detailed Documentation</h1>

---

# Author Information

| Author | Created on | Version | Last updated by | Last edited on | L0 Reviewer | L1 Reviewer | L2 Reviewer |
|---|---|---|---|---|---|---|---|
| Saransh Rai | 01-05-2026 | 1.0 | Saransh Rai | 01-05-2026 | Anuj Jain | Prashant Sharma | Piyush Upadhyay |

---

# Table of Contents

| Section No. | Section Name |
|------------|--------------|
| 1 | [Introduction](#1-introduction) |
| 2 | [Purpose](#2-purpose) |
| 3 | [Key Objectives](#3-key-objectives) |
| 4 | [Pre-Requisites](#4-pre-requisites) |
| 4.1 | [System Requirements](#41-system-requirements) |
| 4.2 | [Dependencies](#42-dependencies) |
| 4.2.1 | [Build Time Dependencies](#421-build-time-dependencies) |
| 4.2.2 | [Run Time Dependencies](#422-run-time-dependencies) |
| 4.2.3 | [Other Dependencies](#423-other-dependencies) |
| 4.3 | [Important Ports](#43-important-ports) |
| 4.3.1 | [Inbound Traffic](#431-inbound-traffic) |
| 4.3.2 | [Outbound Traffic](#432-outbound-traffic) |
| 5 | [Architecture](#5-architecture) |
| 5.1 | [Architecture Overview](#51-architecture-overview) |
| 5.2 | [Core Components](#52-core-components) |
| 5.3 | [Request Flow](#53-request-flow) |
| 5.4 | [Caching Mechanism](#54-caching-mechanism) |
| 5.5 | [Database Layer](#55-database-layer) |
| 5.6 | [Monitoring and Metrics](#56-monitoring-and-metrics) |
| 6 | [Dataflow Diagram](#6-dataflow-diagram) |
| 6.1 | [Data Flow Explanation](#61-data-flow-explanation) |
| 7 | [Application Startup Workflow](#7-application-startup-workflow) |
| 8 | [Health Check](#8-health-check) |
| 9 | [Logging and Monitoring](#9-logging-and-monitoring) |
| 10 | [Troubleshooting](#10-troubleshooting) |
| 11 | [Best Practices](#11-best-practices) |
| 12 | [FAQs](#12-faqs) |
| 13 | [How to Bring Up the Salary API](#13-how-to-bring-up-the-salary-api) |
| 14 | [Contact Information](#14-contact-information) |
| 15 | [References](#15-references) |

---

# 1. Introduction

The Salary API is a Java-based microservice developed as part of the OT-Microservices ecosystem to manage salary-related operations independently and efficiently.

The service is built using Spring Boot and follows a microservices architecture pattern, allowing independent deployment, scaling, monitoring, and maintenance without affecting other services in the platform.

The Salary API primarily handles salary record management, employee salary retrieval, and salary-related business operations while integrating with ScyllaDB for distributed data storage and Redis for high-speed caching.

The service exposes REST-based APIs for client applications and other internal services to interact with salary data securely and efficiently.

The architecture is designed to provide:

* High availability
* Faster response time
* Fault isolation
* Independent scalability
* Better maintainability
* Improved operational visibility

The application also includes monitoring and health check capabilities using Spring Boot Actuator and Prometheus-compatible metrics for operational monitoring.

---

# 2. Purpose

The purpose of the Salary API is to provide a centralized and scalable salary management service within the OT-Microservices platform.

It separates salary operations from other business services, enabling independent development, deployment, scaling, and maintenance.

The service is designed to improve performance, reduce database load using caching mechanisms, and ensure reliable salary data management through distributed database architecture.

---

# 3. Key Objectives

The major objectives of the Salary API are:

* Provide centralized salary management functionality
* Enable independent deployment of salary-related services
* Improve application scalability using microservices architecture
* Reduce response time using Redis caching
* Ensure reliable distributed storage using ScyllaDB
* Improve fault isolation between services
* Support horizontal scalability
* Enable easier troubleshooting and maintenance
* Improve observability using health checks and metrics
* Provide REST-based APIs for frontend and internal services

---

# 4. Pre-Requisites

Before deploying the Salary API, ensure that the following hardware, software, and security requirements are met.

---

# 4.1 System Requirements

## Hardware Specifications

| Hardware | Minimum Recommendation |
|---|---|
| Processor | Dual-core |
| RAM | 4 GB |
| Disk | 20 GB |
| OS | Ubuntu 22.04 |

---

# 4.2 Dependencies

## 4.2.1 Build Time Dependencies

| Name | Version | Description |
|---|---|---|
| Java (JDK) | 11 or above | Required to compile the application |
| Maven | 3.x | Build and dependency management |

---

## 4.2.2 Run Time Dependencies

| Name | Version | Description |
|---|---|---|
| Java Runtime (JRE) | 11 or above | Required to run the application |
| ScyllaDB | Cassandra compatible | Primary database for salary data |
| Redis | Latest stable | Cache management |
| Migrate | Latest | Database schema migration |

---

## 4.2.3 Other Dependencies

| Name | Version | Description |
|---|---|---|
| Prometheus | Optional | Metrics scraping |
| Swagger UI | Bundled | API documentation and testing |

---

# 4.3 Important Ports

## 4.3.1 Inbound Traffic

| Port | Description |
|---|---|
| 9042 | Used by ScyllaDB |

---

## 4.3.2 Outbound Traffic

| Port | Description |
|---|---|
| 8082 | Used by embedded Tomcat server |

---

# 5. Architecture

## 5.1 Architecture Overview

The Salary API follows a microservices-based architecture where all salary-related operations are handled independently by a dedicated Spring Boot service.

The service communicates over HTTP/REST protocols and integrates with Redis for caching and ScyllaDB for persistent distributed storage.

This architecture helps achieve:

* Independent scalability
* Faster deployments
* Better fault isolation
* Improved maintainability
* High availability

---

## 5.2 Core Components

| Component | Description |
|---|---|
| Client / Frontend | Sends salary-related API requests |
| Salary API | Main Spring Boot microservice handling business logic |
| Redis | In-memory caching layer for faster response |
| ScyllaDB | Distributed NoSQL database storing salary records |
| Prometheus | Metrics collection and monitoring |
| Spring Boot Actuator | Health checks and operational endpoints |

---

## 5.3 Request Flow

1. Client sends an HTTP request to the Salary API
2. Salary API validates and processes the request
3. Redis cache is checked for existing data
4. If cache hit occurs, response is returned immediately
5. If cache miss occurs:
   * Data is fetched from ScyllaDB
   * Retrieved data is cached in Redis
   * Response is returned to the client

---

## 5.4 Caching Mechanism

Redis is used as a caching layer to improve application performance and reduce direct database calls.

### Advantages of Caching

* Faster response time
* Reduced database load
* Better scalability
* Improved user experience

The API first checks Redis before querying ScyllaDB.

---

## 5.5 Database Layer

ScyllaDB is used as the primary distributed database for salary data storage.

ScyllaDB is Cassandra-compatible and provides:

* High throughput
* Horizontal scalability
* Fault tolerance
* Distributed storage
* Low latency operations

The application communicates with ScyllaDB using Cassandra-compatible drivers.

---

## 5.6 Monitoring and Metrics

The Salary API exposes health and metrics endpoints using Spring Boot Actuator.

Prometheus can scrape metrics from the application for monitoring purposes.

Monitoring helps in:

* Service health tracking
* Performance monitoring
* Error analysis
* Capacity planning
* Operational troubleshooting

---

<details>
<summary><strong>📌 Salary API Architecture Diagram</strong></summary>

<br>

<img width="1692" height="930" alt="image" src="https://github.com/user-attachments/assets/264fe8b3-5654-4195-be32-a7e7313e78a0" />

</details>

---

# 6. Dataflow Diagram

<details>
<summary><strong>📌 Salary API Dataflow Diagram</strong></summary>

<br>

<img width="3362" height="500" alt="mermaid-diagram (2)" src="https://github.com/user-attachments/assets/83d75c66-878d-4f2e-9a3a-7b0e1bfd59d7" />

</details>

---

## 6.1 Data Flow Explanation

1. Client sends an HTTP request to the Salary API
2. Salary API checks Redis cache for existing data
3. If cache hit occurs, response is returned immediately
4. If cache miss occurs, data is fetched from ScyllaDB
5. Retrieved data is stored in Redis cache
6. API returns the response to the client

---

# 7. Application Startup Workflow

The following sequence occurs when the Salary API application starts:

1. Java Virtual Machine (JVM) initializes
2. Spring Boot application starts
3. Application configuration files are loaded
4. Embedded Tomcat server initializes
5. Database connection with ScyllaDB is established
6. Redis cache connection initializes
7. Spring Beans are loaded
8. API routes and controllers are initialized
9. Health check endpoints become available
10. Application starts accepting client requests

---

# 8. Health Check

Run the following command to verify whether the Salary API application is running properly:

```bash
curl http://localhost:8082/actuator/health
```

## Expected Output

```json
{"status":"UP","components":{"cassandra":{"status":"UP"}}}
```

This confirms that:

* Salary API is running successfully
* Spring Boot application is healthy
* ScyllaDB connectivity is working properly

---

# 9. Logging and Monitoring

The Salary API provides operational logging and monitoring capabilities for troubleshooting and observability.

## Application Logs

Logs help identify:

* Startup failures
* Runtime exceptions
* Database connectivity issues
* Cache failures
* API processing errors

### View Logs

```bash
tail -f ~/salary.log
```

---

## Health Monitoring

Health checks are exposed using Spring Boot Actuator.

### Health Endpoint

```bash
curl http://localhost:8082/actuator/health
```

---

## Metrics Monitoring

Prometheus-compatible metrics can be exposed for:

* API response time
* Request count
* JVM memory usage
* CPU utilization
* Database metrics

---

# 10. Troubleshooting

| Issue | Possible Cause | Resolution | Command |
|---|---|---|---|
| Application not starting | Java missing or incorrect version | Verify Java installation | `java -version` |
| Maven build failure | Dependency issue | Rebuild application | `mvn clean install` |
| ScyllaDB connection failure | Database service down | Verify ScyllaDB status | `sudo systemctl status scylla-server` |
| Redis connection error | Redis service unavailable | Verify Redis service | `sudo systemctl status redis` |
| Health endpoint DOWN | DB or Redis issue | Verify dependent services | `curl http://localhost:8082/actuator/health` |
| Port already in use | Another process using the port | Identify running process | `sudo ss -ltnp` |
| Application logs not visible | Incorrect log path | Verify log file | `tail -f ~/salary.log` |
| Slow API response | Cache miss or DB latency | Verify Redis and DB performance | `redis-cli ping` |

---

# 11. Best Practices

| Best Practice | Description |
|---|---|
| Use environment variables | Avoid hardcoding credentials |
| Enable monitoring | Use Prometheus and Actuator |
| Use Redis caching properly | Reduce unnecessary DB calls |
| Monitor JVM memory | Prevent memory-related crashes |
| Rotate application logs | Prevent disk space exhaustion |
| Use health checks | Validate service availability |
| Perform regular backups | Protect salary data |
| Restrict open ports | Improve security posture |
| Use version-controlled migrations | Maintain schema consistency |

---

# 12. FAQs

### Question: Why is Redis used in the Salary API?

Redis is used to cache frequently accessed salary data, reducing database load and improving response time.

---

### Question: Why is ScyllaDB used instead of traditional SQL databases?

ScyllaDB provides distributed architecture, high throughput, and better scalability for microservices environments.

---

### Question: Can the Salary API scale horizontally?

Yes. Since the application follows a stateless microservices architecture, multiple instances can run behind a load balancer.

---

### Question: How is application health monitored?

The application exposes Spring Boot Actuator endpoints that can be monitored using Prometheus and monitoring dashboards.

---

### Question: Which protocol is used for communication?

The application communicates using HTTP/REST APIs.

---

### Question: What happens during a cache miss?

If data is not available in Redis cache, the application fetches it from ScyllaDB and stores it back into Redis.

---

# 13. How to Bring Up the Salary API

To set up and run this application, refer to the official repository documentation:

👉 [Salary POC README Documentation](https://github.com/Snaatak-Infra-Titans/Documentations/blob/SCRUM-72-versha/OT_MS_Understanding/POC/Salary/README.md)

---

# 14. Contact Information

| Role | Name | Email |
|---|---|---|
| Author / Owner | Saransh Rai | saransh.rai.snaatak@mygurukulam.co |
| POC (Setup) | Versha Tripathi | versha.tripathi.snaatak@mygurukulam.co |

---

# 15. References

| Topic | Description |
|---|---|
| [Jenkins Installation Documentation](https://www.jenkins.io/doc/book/installing/linux/#debianubuntu) | Jenkins installation and Linux setup reference |
| [FAQ Structure Reference](https://amplifi.com/user-guide/FAQs.html) | FAQ documentation structure reference |
| [Introduction vs Overview Reference](https://thecontentauthority.com/blog/introduction-vs-overview) | Guidance for writing introduction sections |
| [Salary POC GitHub README](https://github.com/Snaatak-Infra-Titans/Documentations/blob/SCRUM-72-versha/OT_MS_Understanding/POC/Salary/README.md) | Salary API implementation and setup reference |
