
# 🛍️ E-Commerce Backend Microservices Platform

## 🌟 Overview

The **E-Commerce Backend Microservices Platform** is a scalable, containerized backend system built using a modern microservices architecture. It is designed to provide flexibility, maintainability, and high availability for an e-commerce ecosystem.

The system leverages:

- **Spring Boot & Spring Cloud** for service development
- **Apache Kafka** for asynchronous communication
- **Eureka** for service discovery
- **Spring Cloud Gateway** as API Gateway
- **Redis** for caching
- **MySQL** for persistent storage
- **Zipkin** for distributed tracing
- **Docker & Docker Compose** for container orchestration

Each microservice is independently deployable and fully containerized, enabling seamless local development and production-ready deployment.

---

# 🏗️ Microservices Architecture

The platform consists of the following core services:

---

## 1️⃣ API Gateway

**Purpose:**  
Acts as the centralized entry point for all client requests.

**Technology:** Spring Cloud Gateway  

**Responsibilities:**
- Request routing
- Load balancing
- Authentication and authorization enforcement
- Rate limiting
- Cross-cutting concerns handling

---

## 2️⃣ Service Registry (Eureka Server)

**Purpose:**  
Provides dynamic service discovery and registration.

**Technology:** Spring Cloud Netflix Eureka  

**Responsibilities:**
- Service registration
- Instance health monitoring
- Dynamic lookup of service endpoints

---

## 3️⃣ Product Service

**Purpose:**  
Manages product catalog and inventory data.

**Technology Stack:**
- Spring Boot
- MySQL
- Redis (Caching Layer)

**Capabilities:**
- Product CRUD operations
- Inventory management
- Performance optimization using caching

---

## 4️⃣ Order Service

**Purpose:**  
Handles order lifecycle management.

**Technology Stack:**
- Spring Boot
- MySQL
- Zipkin (Tracing)

**Capabilities:**
- Order creation and validation
- Order status tracking
- Order history retrieval
- Distributed request tracing

---

## 5️⃣ Email Service

**Purpose:**  
Handles system notifications and communication.

**Technology Stack:**
- Spring Boot
- MySQL

**Capabilities:**
- Order confirmation emails
- Password reset notifications
- Scheduled email dispatch
- Delivery tracking

---

## 6️⃣ Identity Service

**Purpose:**  
Responsible for authentication and authorization.

**Technology Stack:**
- Spring Boot
- MySQL
- Redis

**Capabilities:**
- User registration & login
- Role-based access control (RBAC)
- JWT token generation
- Session management

---

## 7️⃣ Payment Service

**Purpose:**  
Manages payment processing and transaction history.

**Technology Stack:**
- Spring Boot
- MySQL
- Zipkin

**Capabilities:**
- Payment transaction processing
- Invoice generation
- Transaction history management
- Distributed tracing

---

# 🔧 Technology Stack

| Category | Technologies |
|----------|--------------|
| Backend Framework | Spring Boot |
| Service Discovery | Eureka |
| API Gateway | Spring Cloud Gateway |
| Messaging | Apache Kafka |
| Inter-Service Calls | OpenFeign |
| Database | MySQL |
| Caching | Redis |
| Observability | Zipkin |
| Containerization | Docker |
| Orchestration | Docker Compose |

---

# 🚀 Getting Started

## ✅ Prerequisites

Ensure the following are installed:

- Docker
- Docker Compose
- Git
- Web browser
- MySQL client (optional)

---

## 📥 Clone the Repository

```bash
git clone https://github.com/haphong463/springboot-kafka-microservices.git
cd springboot-kafka-microservices
```

---

## 🐳 Run the Entire System

Start all services using Docker Compose:

```bash
docker-compose up -d
```

To verify running containers:

```bash
docker-compose ps
```

---

# 🔌 Service Access Points

| Service | URL |
|----------|------|
| API Gateway | http://localhost:9191 |
| Eureka Dashboard | http://localhost:8761 |
| Zipkin UI | http://localhost:9411 |
| Order Service | http://localhost:8080 |
| Product Service | http://localhost:8084 |
| Payment Service | http://localhost:8085 |
| Email Service | http://localhost:8081 |
| Identity Service | http://localhost:9898 |

---

# 🔐 Authentication & API Usage

## User Registration

```bash
curl -X POST http://localhost:9191/api/v1/auth/register \
-H "Content-Type: application/json" \
-d '{
  "name": "johndoe",
  "password": "securepassword",
  "email": "john.doe@example.com",
  "roles": ["CUSTOMER"]
}'
```

---

## User Login

```bash
curl -X POST http://localhost:9191/api/v1/auth/token \
-H "Content-Type: application/json" \
-d '{
  "username": "johndoe",
  "password": "securepassword"
}'
```

The server returns a JWT token (stored in cookies) for authenticated requests.

---

## Example: Add Product (EMPLOYEE Role Required)

```bash
curl -X POST http://localhost:9191/api/v1/products \
-H "Content-Type: application/json" \
-b "token=your_jwt_token" \
-d '{
  "name": "New Product",
  "imageUrl": "image1.png",
  "description": "Product description",
  "price": 99.99,
  "stockQuantity": 100
}'
```

---

## Example: Create Order (CUSTOMER Role Required)

```bash
curl -X POST http://localhost:9191/api/v1/order \
-H "Content-Type: application/json" \
-b "token=your_jwt_token" \
-d '{
  "orderItems": [{
      "productId": "UUID",
      "quantity": 1
  }],
  "paymentMethod": "COD"
}'
```

---

# 🗄️ Database Management

Each microservice maintains its own dedicated MySQL database to ensure data isolation and service independence.

### Connect from Host

```bash
mysql -h 127.0.0.1 -P 3307 -u root -p
```

### Connect Inside Container

```bash
docker exec -it mysql-order-service bash
mysql -u root -proot order_db
```

---

# 🛠️ Troubleshooting

### Services Not Registering in Eureka
- Verify `EUREKA_CLIENT_SERVICEURL_DEFAULTZONE`
- Ensure Docker network connectivity
- Check service logs

---

### Kafka Port Conflicts
Ensure unique listener ports:
```yaml
KAFKA_ADVERTISED_LISTENERS:
PLAINTEXT://kafka:9092,
PLAINTEXT_HOST://localhost:29092
```

---

### MySQL Volume Errors
Ensure volumes are declared in `docker-compose.yml`.

---

### Check Service Logs
```bash
docker-compose logs -f <service-name>
```

---

### Rebuild Specific Service
```bash
docker-compose up -d --build order-service
```

---

# 📊 Observability & Monitoring

- **Zipkin UI** enables tracing across distributed services.
- Helps analyze latency, request flow, and bottlenecks.
- Enables debugging of inter-service communication.


