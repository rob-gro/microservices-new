# Microservices E-Commerce Platform

Microservices E-Commerce Platform is a comprehensive online shopping system built with Spring Boot microservices architecture. The application leverages modern cloud-native patterns to create a robust, scalable, and resilient e-commerce solution.

## 🚀 Features

### 🛍️ Core E-Commerce Functionality
- **Product Management**: Create and browse products
- **Inventory Management**: Track stock levels for products
- **Order Processing**: Place orders with stock verification
- **Distributed Catalog**: Products and inventory managed separately

### 🔧 Technical Capabilities
- **Service Discovery**: Dynamic service registration and discovery
- **API Gateway**: Centralized routing and security
- **Circuit Breakers**: Resilience against service failures
- **Load Balancing**: Distribute traffic among service instances
- **Distributed Tracing**: Track requests across services
- **Security**: OAuth2 resource server protection

## 🏗️ Architecture Overview

The system is built as a set of interconnected microservices:

```
┌─────────────────┐     ┌─────────────────┐
│                 │     │                 │
│   API Gateway   │────▶│ Discovery Server│
│                 │     │     (Eureka)    │
└────────┬────────┘     └─────────────────┘
         │
         │
         ▼
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│                 │     │                 │     │                 │
│ Product Service │◀───▶│  Order Service  │────▶│Inventory Service│
│   (MongoDB)     │     │    (MySQL)      │     │    (MySQL)      │
└─────────────────┘     └─────────────────┘     └─────────────────┘
```

## 🛠 Technologies

- **Framework**: Spring Boot, Spring Cloud
- **Service Discovery**: Netflix Eureka
- **API Gateway**: Spring Cloud Gateway
- **Database**:
    - MongoDB for Product Service
    - MySQL for Order and Inventory Services
- **Resilience**: Resilience4j (Circuit Breaker, Retry, Time Limiter)
- **Client Communication**: WebClient with Load Balancing
- **Security**: OAuth2 Resource Server, JWT
- **Distributed Tracing**: Zipkin, Sleuth
- **Build Tool**: Maven

## 🗂 Project Structure

### Microservices

#### API Gateway
- Central entry point for all client requests
- Routes requests to appropriate microservices
- Implements OAuth2 security for all services
- Configuration in application.properties for routing rules

#### Discovery Server
- Eureka server for service registration and discovery
- Secures Eureka dashboard with basic authentication
- Allows services to find each other without hardcoded URLs

#### Product Service
- Manages product catalog (creation, retrieval)
- Uses MongoDB for product data storage
- Provides REST API for product operations
- Registers with Eureka for service discovery

#### Inventory Service
- Tracks product inventory levels
- Checks if products are in stock
- Uses MySQL for inventory data storage
- Provides internal API for inventory verification

#### Order Service
- Handles order processing
- Communicates with Inventory Service to verify stock
- Implements resilience patterns (Circuit Breaker, Retry, Time Limiter)
- Uses MySQL for order data storage

## 🔄 Service Interaction

1. **Order Flow**:
    - Order Service receives order request
    - Calls Inventory Service to check stock availability
    - If stock is available, creates order
    - Returns confirmation to client
    - Circuit breaker triggers fallback if Inventory Service is unavailable

2. **Product Catalog**:
    - Product Service handles product creation and browsing
    - Clients access products through API Gateway

3. **Service Discovery**:
    - All services register with Eureka Discovery Server
    - Services locate each other via Eureka
    - Load balancing between service instances

## 🔐 Security

- API Gateway secures all endpoints with OAuth2 Resource Server
- JWT tokens required for authenticated requests
- Eureka Dashboard protected with basic authentication
- Secure service-to-service communication

## 🚀 Running the Application

### Prerequisites
- Java 11+
- Maven
- MySQL
- MongoDB
- Keycloak (for OAuth2)
- Zipkin

### Setup Steps
1. Start MySQL and create databases:
    - `order-service`
    - `inventory-service`

2. Start MongoDB for product-service

3. Start Zipkin:
   ```
   docker run -d -p 9411:9411 openzipkin/zipkin
   ```

4. Start Keycloak and configure realm:
    - Create `spring-boot-microservices-realm`
    - Setup clients and users

5. Start services in order:
    - Discovery Server
    - API Gateway, Product, Order, and Inventory services

## 📡 Service Endpoints

- **Product Service**: `/api/product`
    - POST - Create product
    - GET - Retrieve all products

- **Order Service**: `/api/order`
    - POST - Place order

- **Inventory Service** (Internal): `/api/inventory`
    - GET - Check inventory status

- **Discovery Server**:
    - `/eureka/web` - Eureka Dashboard

## 🔧 Resilience Features

- **Circuit Breaker**: Prevents cascading failures when inventory service is down
- **Retry Mechanism**: Automatically retries failed inventory checks
- **Time Limiter**: Sets maximum duration for inventory service calls
- **Fallback Responses**: Graceful degradation when services are unavailable

## 📊 Monitoring and Tracing

- Distributed tracing with Zipkin and Sleuth
- Trace requests across microservice boundaries
- Circuit breaker metrics via Actuator endpoints
- Health indicators for all services

---

Microservices E-Commerce Platform demonstrates modern cloud-native application development practices, focusing on resilience, scalability, and maintainability through microservices architecture patterns.
