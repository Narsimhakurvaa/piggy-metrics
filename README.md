# PiggyMetrics — Microservices Financial Management Platform

A **Spring Boot and Spring Cloud based microservices application** for managing personal financial data such as accounts, income, expenses, savings, statistics, and notification preferences.

This repository is maintained as a **portfolio and learning project** focused on understanding distributed systems, microservice communication, service discovery, API gateways, authentication, resilience, monitoring, Docker, and MongoDB.

> **Attribution:** This project is based on the open-source **PiggyMetrics** project originally created by `sqshq`. The original license and project attribution are retained. This repository is being maintained and customized for portfolio and learning purposes.

---

## 🚀 Project Overview

PiggyMetrics demonstrates how a financial management application can be decomposed into independently deployable services.

The application follows a **microservices architecture**, with dedicated services for business functionality and infrastructure concerns.

### High-Level Architecture

```text
                         ┌──────────────────────┐
                         │       Client         │
                         │      / Web UI        │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │      API Gateway     │
                         └──────────┬───────────┘
                                    │
                ┌───────────────────┼───────────────────┐
                │                   │                   │
                ▼                   ▼                   ▼
       ┌────────────────┐  ┌────────────────┐  ┌────────────────┐
       │ Account        │  │ Statistics     │  │ Notification   │
       │ Service        │  │ Service        │  │ Service        │
       └───────┬────────┘  └───────┬────────┘  └───────┬────────┘
               │                   │                   │
               └───────────────────┼───────────────────┘
                                   │
                                   ▼
                         ┌──────────────────────┐
                         │       MongoDB        │
                         └──────────────────────┘

        ┌─────────────────────┐       ┌─────────────────────┐
        │   Eureka Registry   │       │   Config Service    │
        │   Service Discovery │       │ Centralized Config  │
        └─────────────────────┘       └─────────────────────┘

                         ┌──────────────────────┐
                         │      Monitoring      │
                         │  Turbine / Hystrix  │
                         └──────────────────────┘
```

---

## 🧩 Microservices

| Service                    | Responsibility                                                              |
| -------------------------- | --------------------------------------------------------------------------- |
| **Account Service**        | Manages account information, income, expenses, savings and account settings |
| **Statistics Service**     | Calculates financial statistics and maintains time-series data              |
| **Notification Service**   | Manages notification preferences and scheduled notifications                |
| **Auth Service**           | Handles authentication and authorization                                    |
| **API Gateway**            | Central entry point and request routing                                     |
| **Config Service**         | Centralized application configuration                                       |
| **Registry**               | Service discovery using Eureka                                              |
| **Monitoring**             | Application monitoring and dashboard                                        |
| **Turbine Stream Service** | Aggregates service metrics                                                  |

---

## 🛠️ Technology Stack

### Backend

* Java
* Spring Boot
* Spring MVC
* Spring Cloud
* REST APIs
* Spring Cloud Config
* Netflix Eureka
* OpenFeign
* Ribbon
* Hystrix

### Database

* MongoDB

### Messaging & Infrastructure

* RabbitMQ
* Docker
* Docker Compose

### Monitoring & Observability

* Turbine
* Hystrix Dashboard
* Centralized logging concepts
* Distributed tracing concepts

### Build

* Apache Maven

---

## 🔐 Authentication & Authorization

The application separates authentication responsibilities into a dedicated authentication service.

The architecture supports OAuth2-based authorization for:

* User authentication
* Service-to-service communication
* Protected backend resources
* Scope-based access control

Protected resources can use authorization rules such as:

```java
@PreAuthorize("#oauth2.hasScope('server')")
```

---

## 🌐 API Gateway

The API Gateway acts as the **single external entry point** into the application.

Responsibilities include:

* Request routing
* Service location through service discovery
* Authentication integration
* Backend service access
* Centralized request handling

Example route:

```text
/notifications/**
        ↓
Notification Service
```

---

## 🔎 Service Discovery

The application uses **Netflix Eureka** for service discovery.

Instead of hardcoding service addresses, microservices register themselves with the Eureka registry.

```text
Account Service
       │
       ├──────────┐
       ▼          │
 Statistics       │
 Service          │
       │          ▼
       └──────► Eureka Registry
                    ▲
                    │
             Notification
                Service
```

Eureka Dashboard:

```text
http://localhost:8761
```

---

## ⚡ Resilience & Inter-Service Communication

The project demonstrates distributed-system patterns including:

### OpenFeign

Declarative HTTP clients are used for communication between microservices.

```java
@FeignClient(name = "statistics-service")
public interface StatisticsServiceClient {
    // REST communication
}
```

### Load Balancing

Ribbon provides client-side load-balancing capabilities integrated with service discovery.

### Circuit Breaker

Hystrix demonstrates the Circuit Breaker pattern to prevent cascading failures when a downstream service becomes slow or unavailable.

---

## ⚙️ Centralized Configuration

The Config Service provides centralized configuration for the microservices.

Example:

```yaml
spring:
  application:
    name: notification-service

  cloud:
    config:
      uri: http://config:8888
      fail-fast: true
```

This allows services to retrieve configuration without embedding every configuration value directly inside each application.

---

## 🗄️ Data Architecture

Each business service follows a separated-data approach.

```text
Account Service       → MongoDB
Statistics Service   → MongoDB
Notification Service → MongoDB
```

Services communicate through APIs rather than directly accessing another service's persistence layer.

This helps maintain clear service boundaries.

---

## 📊 Monitoring

The project includes monitoring infrastructure based on:

* Hystrix
* Turbine
* Hystrix Dashboard
* Spring Cloud Bus
* RabbitMQ

The monitoring service aggregates information from the distributed services and provides visibility into service behaviour.

Hystrix Dashboard:

```text
http://localhost:9000/hystrix
```

---

## 📝 Logging & Distributed Tracing

The project demonstrates concepts for observing distributed applications through:

* Centralized logging
* Request tracing
* `traceId`
* `spanId`
* Service-specific logging

These identifiers make it easier to follow a request as it travels through multiple services.

---

## 🐳 Docker

The project includes Docker Compose configurations for running the distributed application.

### Development

```bash
docker-compose -f docker-compose.yml -f docker-compose.dev.yml up
```

### Production-style startup

```bash
docker-compose up
```

---

## ▶️ Running the Project

### Prerequisites

Install:

* Java
* Apache Maven
* Docker
* Docker Compose

### Build

```bash
mvn package -DskipTests
```

### Start the development environment

```bash
docker-compose -f docker-compose.yml -f docker-compose.dev.yml up
```

The complete environment requires multiple Spring Boot services, MongoDB instances, and RabbitMQ.

---

## 🌐 Important Endpoints

| Component           | URL                             |
| ------------------- | ------------------------------- |
| API Gateway         | `http://localhost:80`           |
| Eureka Dashboard    | `http://localhost:8761`         |
| Hystrix Dashboard   | `http://localhost:9000/hystrix` |
| RabbitMQ Management | `http://localhost:15672`        |

---

## 📁 Repository Structure

```text
piggy-metrics/
│
├── account-service/
├── auth-service/
├── config/
├── gateway/
├── mongodb/
├── monitoring/
├── notification-service/
├── registry/
├── statistics-service/
├── turbine-stream-service/
│
├── docker-compose.yml
├── docker-compose.dev.yml
├── pom.xml
├── .gitignore
└── README.md
```

---

## 🎯 Key Concepts Demonstrated

This project provides hands-on exposure to:

* Microservices Architecture
* Spring Boot
* Spring Cloud
* REST API development
* API Gateway
* Service Discovery
* Eureka
* OpenFeign
* Client-side Load Balancing
* Circuit Breaker Pattern
* OAuth2 concepts
* MongoDB
* RabbitMQ
* Docker
* Docker Compose
* Centralized Configuration
* Distributed Monitoring
* Distributed Tracing
* Maven

---

## 🔧 Portfolio Development Roadmap

The repository can be further modernized through:

* [ ] Modernize outdated Spring Cloud components
* [ ] Improve automated test coverage
* [ ] Add GitHub Actions CI/CD
* [ ] Add OpenAPI / Swagger documentation
* [ ] Improve application security configuration
* [ ] Improve Docker configuration
* [ ] Add health checks and observability
* [ ] Add architecture diagrams
* [ ] Add project screenshots
* [ ] Improve error handling
* [ ] Add integration tests
* [ ] Document API examples

---

## 👨‍💻 Developer

**Narsimhakurvaa**

GitHub:
https://github.com/Narsimhakurvaa

Repository:
https://github.com/Narsimhakurvaa/piggy-metrics

---

## 📄 Attribution

This repository is based on the open-source **PiggyMetrics** project originally created by `sqshq`.

The original project provided the foundational application, architecture and implementation. This repository is maintained as a portfolio/learning project, with customization and future modernization work documented separately.

The original project license is retained in this repository.

---

## ⭐ Project Status

**Portfolio Development — In Progress**

The goal of this repository is to progressively modernize, test, document and improve the application while demonstrating practical Java and microservices engineering skills.
