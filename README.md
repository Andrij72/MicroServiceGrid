# 🧩 Microservice Grid

**Microservice Grid** — a practical microservices project built with a modern Java-based tech stack.
The goal of the project is to demonstrate a fully working distributed system with testing, CI/CD, and observability.

---

## ⚙️ Tech Stack

- **Java 21 / Spring Boot 3**
- **Spring WebFlux / Reactive Gateway**
- **MongoDB / MySQL / PostgreSQL**
- **Apache Kafka**
- **Spring Cloud API Gateway**
- **Spring Security / Keycloak / OAuth2**
- **Resilience4J (CircuitBreaker / RateLimiter / Bulkhead)**
- **Prometheus / Grafana / Loki / Tempo**
- **Docker / Docker Hub**
- **Kubernetes (planned)**
- **GitHub Actions (CI/CD)**
- **Testcontainers (Integration Tests)**

---

## 🧠 Services Overview

| Service | Description | Status | Repository |
|----------|--------------|---------|-------------|
| **Product Service** | Manages product catalog | ✅ Implemented | [link](https://github.com/Andrij72/product-service) |
| **Order Service** | Handles customer orders | ✅ Implemented | [link](https://github.com/Andrij72/order-service) |
| **Inventory Service** | Tracks product stock levels | 🚧 In progress | [link](https://github.com/Andrij72/inventory-service) |
| **Notification Service** | Sends notifications (Email / Viber) |✅ Implemented| [link](https://github.com/Andrij72/notification-service) |
| **API Gateway** | Central reactive entry point (Spring WebFlux) | ✅ Implemented | [link](https://github.com/Andrij72/gateway) |
| **Auth Server** | Authentication & Authorization (Keycloak / OAuth2) | ✅ Implemented | - |
| **Pay Service** | Payment and currency operations | 🕓 Planned | - |
| **Weather Service** | Weather API integration via Gateway | 🕓 Planned | - |
| **Currency Service** | Currency rates provider | 🕓 Planned | - |

---

## 🧩 Architecture Overview

- **API Gateway** – single reactive entry point to all microservices.
- **Event-driven communication** – via **Kafka** between Product and Order services.
- **Synchronous REST calls** – secured and protected with **Resilience4J CircuitBreaker** and **RateLimiter**.
- **Observability Stack** – every service exports metrics to **Prometheus** and **Grafana** dashboards.
- **Centralized Logging** – using **Loki + Tempo** stack for distributed tracing.

---
## ⚙️ Run & Observe Locally

To start the full system locally:

```bash
git clone https://github.com/yourusername/MicroserviceGrid.git
cd MicroserviceGrid
docker-compose -f docker-compose.orchestrator.yml up --build  -d
```
----
## 🧰 API Testing (Postman Collection)

A complete Postman Collection is included in the project for manual and automated testing of microservices via Gateway and direct endpoints.

📁 **File:** [`MicroServiceGrid.postman_collection.json`](./MicroServiceGrid.postman_collection.json)

### 🔹 Main Endpoints

| Method | Endpoint | Description |
|---------|-----------|-------------|
| `GET` | `/api/v1/products` | Get all products |
| `POST` | `/api/v1/products` | Create new product |
| `POST` | `/api/v1/products/batch` | Create multiple products |
| `GET` | `/api/v1/products/search?name=` | Find product by name |
| `DELETE` | `/api/v1/products/{id}` | Delete product |
| `POST` | `/api/v1/order` | Create new order |
| `GET` | `/api/v1/inventory?skuCode=&quantity=` | Check inventory availability |
| `GET` | `/actuator/health` | Health check for API Gateway |

---

## 🚀 CI/CD and Deployment

- **Docker images** built and pushed to **Docker Hub**.
- **GitHub Actions** automate building, testing, and publishing.
- **Docker Compose** used for local development.
- **Kubernetes** (planned) for orchestration and scalability.

---

## 🌍 Future Extensions

- Email / Telegram notification service
- Currency and crypto tracking microservice
- Weather data integration
- Payment microservice with external APIs

---

## 🎯 Purpose

This project demonstrates:
- Clean **microservice architecture**
- **Reactive programming** with Spring WebFlux
- **Kafka-based event-driven communication**
- **Resilience4J** for fault tolerance
- **CI/CD automation**
- **Monitoring & observability** best practices

---

👤 **Author:** [Andrii Kulynch](https://github.com/Andrij72)
📅 **Version:** 1.0
