# 🧩 Microservice Grid 🧩       

----
**MicroserviceGrid** is a full-fledged microservices system built on **Spring Boot 3 / Spring Cloud**, featuring a reactive entry point, asynchronous communication, fault-tolerance, monitoring, and centralized management via **Docker Compose**.

This repository serves as the **orchestrator** for the entire system and includes:

- **Docker Compose configurations** for all microservices
- **Observability stack** (Prometheus, Grafana, Loki, Tempo)
- **Keycloak** for authentication and authorization
- **Apache Kafka** for event-driven communication
- **Single network** shared by all services
---

## ⚙️ Tech Stack

- **Java 21 / Spring Boot 3**
- **Spring WebFlux / Reactive Gateway**
- **MongoDB / MySQL / PostgreSQL**
- **Apache Kafka**
- **Spring Cloud API Gateway**
- **Spring Security / Keycloak / OAuth2**
- **Resilience4J (CircuitBreaker / RateLimiter / Bulkhead)**
- **Prometheus / Grafana / Loki / Tempo**lkhead)**
- **Docker / Docker Hub**
- **Kubernetes (planned)**
- **GitHub Actions (CI/CD)**
- **Testcontainers (Integration Tests)**
- **Angular 20 (Frontend + Admin Panel)**

---
---
##  🧠 Services Overview    
  
   | Service                  | Description                                        | Status         | Repository                                               |
   |--------------------------|----------------------------------------------------|----------------|----------------------------------------------------------|
   | **Product Service**      | Manages product catalog                            | ✅ Implemented  | [link](https://github.com/Andrij72/product-service)      |
   | **Order Service**        | Handles customer orders                            | ✅ Implemented  | [link](https://github.com/Andrij72/order-service)        |
   | **Inventory Service**    | Tracks product stock levels                        | 🚧 In progress | [link](https://github.com/Andrij72/inventory-service)    |
   | **Notification Service** | Sends notifications (Email / Viber)                | ✅ Implemented  | [link](https://github.com/Andrij72/notification-service) |
   | **API Gateway**          | Central reactive entry point (Spring WebFlux)      | ✅ Implemented  | [link](https://github.com/Andrij72/api-gateway)          |
   | **Auth Server**          | Authentication & Authorization (Keycloak / OAuth2) | ✅ Implemented  | -                                                        |
   | **Pay Service**          | Payment and currency operations                    | 🕓 Planned     | -                                                        |
   

---
## 🌐 High-Level Architecture
````text
🅰️ Angular Frontend
  Shop + Admin Panel
       │ HTTP
       ▼
🚪 API Gateway
  Spring Cloud Gateway (WebFlux)
       │
 ┌─────────────┐  ┌─────────────┐
 │🧾Order Svc  │  │📦Product Svc│
 │ MySQL       │  │ MongoDB     │
 │ REST + Kafka│  │ Kafka       │
 └─────┬───────┘  └─────┬───────┘
       │               │
┌──────▼──────┐  ┌─────▼─────────┐
│🏬 Inventory │  │🔔 Notification│
│ MySQL       │  │ Kafka Consumer│
│ REST Sync   │  │ Email / Viber │
└─────────────┘  └───────────────┘
       │
🟠 Kafka Messaging
       │
📊 Observability (Prometheus / Grafana / Loki / Tempo)
🐳 Docker

````
### 🔹 Data Flows

- **Client → API Gateway → Order Service** – REST CRUD requests
- **Order Service → Inventory Service** – synchronous stock check
- **Order Service → Kafka → Notification Service** – order/status events
- **Order Service → MySQL** – store orders and user data
- **Product Service ↔ Kafka ↔ Order Service** – Product Service receives order events
- **Monitoring / Observability** – Prometheus, Grafana, Loki, Tempo

---

  ## 🛍️ Frontend Application – MicroserviceGridShopFrontend
  
  The frontend application of the **Microservice Grid** ecosystem is built with **Angular 20** and serves both the shop and admin panel.  
  It communicates with the API Gateway and backend microservices to provide a modular, reactive, and scalable user interface.
  
  📁 Repository: [MicroserviceGridShopFrontend](https://github.com/Andrij72/MicroserviceGridShopFrontEnd)
  
  ### Key Features
  - Product catalog (Product Service)
  - Inventory availability (Inventory Service)
  - Order creation (Order Service)
  - Admin panel for managing products, orders, and users
  - Secure API integration via API Gateway
  - Docker-ready production build





---

### 🚀 Running the Project

#### 1️⃣ Start Microservices (Locally)

To start the full system locally:
          
```bash
git clone https://github.com/Andrij72/MicroserviceGrid.git
````
```bash
# Go to the project root
cd microservice-grid  
```                   
#### Start all microservices
```bash
docker-compose -f docker-compose.yml up -d
````
2️⃣ Start Observability Stack

In the project root, there is a file docker-compose-observability.yml:
```bash
docker-compose -f docker-compose-observability.yml up -d
```
🔹 Observability Stack Services

| Service    | Host Port   | Purpose                    |
| ---------- | ----------- | -------------------------- |
| Loki       | 3100        | Logging                    |
| Prometheus | 9090        | Metrics                    |
| Tempo      | 3110 / 9411 | Traces / Zipkin            |
| Grafana    | 3000        | Dashboards & Visualization |

   🔹 Network Configuration
 ```yaml
networks:
  microservices-net:
    external: true          
 ```       
- All services are connected to microservices-net
- Grafana depends on Loki, Prometheus, and Tempo via depends_on
- Anonymous access to Grafana is enabled (Admin role)
- Tempo stores data in ./docker/tempo/tempo-data

----
  
 ## 🔐 Authentication & Authorization (Keycloak)
 
 The system uses **Keycloak** as an OAuth2 / OpenID Connect server.
 
 ### Implemented:
 - JWT-based authentication
 - Client Credentials flow (service-to-service)
 - Role-based access control (ADMIN / CLIENT)
 - Integration via Spring Security
 
 ### Keycloak Configuration
 
 The basic Keycloak setup (realm, clients, roles)  
 is documented with screenshots:
 
 📁 [`src/main/resources/static/keycloak/`](src/main/resources/static/keycloak/)
 
 Screenshots demonstrate:
 - Realm creation
 - Clients configuration
 - Roles and mappings
 - Token configuration
 
 > ⚠️ In a production environment, Keycloak configuration  
 > should be done via **realm-export (JSON)** or **Terraform**.  
 > Screenshots are provided **for demonstration and educational purposes only**.
 ----

---
## 🧪🧰 API Testing (Postman Collection)

A complete Postman Collection is included in the project for manual and automated testing of microservices via Gateway and direct endpoints.

📁 **File:** [`MicroServiceGrid.postman_collection.json`](./MicroServiceGrid.postman_collection.json)

  
  The collection covers requests for:
  
  - 🔎 **Health checks**
    - `/actuator/health`
  
  - 📦 **Product Service**
    - Public API: get product by SKU
    - Admin API: CRUD operations, batch operations, pagination
    - Enable / Disable products
  
  - 🧾 **Order Service**
    - Create / Update / Delete orders
    - Order status workflow
    - Pagination, filtering, multi-sort
  
  - 🏬 **Inventory Service**
    - Stock availability checks
    - Quantity validation
  
  - 🔐 **Security**
    - OAuth2 (Client Credentials)
    - Bearer Token flow via Keycloak
  
  > The Postman collection is the **source of truth** for the API.  
  > The README does not duplicate the full list of endpoints on purpose.
  
  ---
  
  ### 🔹 Quick Reference: Main Endpoints
  
  #### Products
  | Method | Endpoint | Description |
  |--------|---------|-------------|
  | `GET` | `/api/v1/products` | Get all products |
  | `GET` | `/api/v1/products/{{sku}}` | Get product by SKU (public) |
  | `POST` | `/api/v1/admin/products` | Create a new product (admin) |
  | `POST` | `/api/v1/admin/products/batch` | Batch create products (admin) |
  | `PUT` | `/api/v1/admin/products/{{sku}}` | Update product (admin) |
  | `PATCH` | `/api/v1/admin/products/{{sku}}/enable` | Enable/disable product (admin) |
  | `DELETE` | `/api/v1/admin/products/batch` | Delete multiple products (admin) |
  
  #### Orders
  | Method | Endpoint | Description |
  |--------|---------|-------------|
  | `POST` | `/api/v1/orders` | Create new order |
  | `GET` | `/api/v1/orders` | Get all orders |
  | `GET` | `/api/v1/orders?page=0&size=10&status=&email=&sort=` | Get orders with pagination & filters |
  | `GET` | `/api/v1/orders/{{orderNumber}}` | Get order by number |
  | `PUT` | `/api/v1/orders/{{orderNumber}}` | Update order (full) |
  | `PATCH` | `/api/v1/orders/{{orderNbr}}/status` | Update order status |
  | `DELETE` | `/api/v1/orders/{{orderNumber}}` | Delete order |
  
  #### Inventory
  | Method | Endpoint | Description |
  |--------|---------|-------------|
  | `GET` | `/api/v1/inventory?skuCode=&quantity=` | Check inventory availability |
  
  #### Health
  | Method | Endpoint | Description |
  |--------|---------|-------------|
  | `GET` | `/actuator/health` | Health check for API Gateway |



---

## 🚀 CI/CD and Deployment

- **Docker images** built and pushed to **Docker Hub**.
- **GitHub Actions** automate building, testing, and publishing.
- **Docker Compose** used for local development.
- **Kubernetes** (planned) for orchestration and scalability.

---

## 🌍 Future Extensions
- Email / Telegram notifications  
- Payment microservice (external APIs)  
- AI analytics: resource usage, order flows, sales

---

## 🎯 Purpose
This project demonstrates:
- Clean microservice architecture with reactive programming
- Kafka-based event-driven communication
- Fault tolerance via Resilience4J
- CI/CD automation and best practices for monitoring & observability
- Full end-to-end production-ready system

---

👤 **Author:** [Andrii Kulynch](https://github.com/Andrij72)

📅 **Version:** 2.0
