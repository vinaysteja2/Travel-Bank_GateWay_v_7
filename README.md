# 🌐 Travel-Bank_Gateway (v7.0) 🚀

> 🚦 Centralized, Reactive API Gateway for the Travel Bank Microservices Suite

This project is the **seventh version** of the **Travel Bank Microservices Suite** and introduces **Spring Cloud Gateway** as the central entry point for all service calls. It continues the journey from the Service Registry (v6.0) and implements modern API gateway patterns, routing, and request filtering capabilities.

---

## 🚪 What is Spring Cloud Gateway?

**Spring Cloud Gateway** is a library that allows you to build a **non-blocking API gateway** using **Spring WebFlux**. It acts as a **gatekeeper** to your microservices by routing, filtering, and securing incoming requests.

### 🔑 Key Features:

✨ Reactive & Non-blocking routing using WebFlux  
🛡 Centralized entry point for all service requests  
🔍 Integration with Eureka for dynamic service discovery  
📬 Header manipulation and correlation ID tracking  
🧱 Filter-based architecture for request/response customization  
⚡ Replaces Netflix Zuul with better performance and support  

---

## 📦 Modules in the Travel Bank Architecture So Far

| Version | 📁 Module               | 🔎 Description                               |
|---------|-------------------------|----------------------------------------------|
| [v1.0](https://github.com/vinaysteja2/TRAVEL-BANK_Micorservices_v_1.git) | Microservices Core     | Base services: accounts, loans, cards       |
| [v2.0](https://github.com/vinaysteja2/TRAVEL-BANK_Docker_v_2.git)        | Dockerization          | Containerization of services                |
| [v3.0](https://github.com/vinaysteja2/Travel-Bank_Config_Management_v_3.git) | Config Management      | Central config via Spring Cloud Config      |
| [v4.0](https://github.com/vinaysteja2/Travel-Bank_ConfigServer_v_4.git)  | Config Server          | Config service hosting shared properties    |
| [v5.0](https://github.com/vinaysteja2/Travel-Bank_MySqlDB_v_5.git)       | MySQL Integration      | Persistent DB support with MySQL            |
| [v6.0](https://github.com/vinaysteja2/Travel-Bank_ServiceRegistry_v6)   | Service Registry       | Eureka-based service discovery              |
| ⭐ **v7.0** | **Gateway**              | **Central gateway using Spring Gateway**     |

---

## 🧠 How Gateway Works

### 📐 Architecture:
```
Client 🧑‍💻 → Gateway 🌐 → Eureka Discovery 🔎 → Microservices 🧩
```

### 🔁 Request Lifecycle:

1️⃣ **Incoming request** arrives at the Gateway  
2️⃣ **RequestTraceFilter** checks for a correlation ID  
   🔹 If absent, it generates one and adds it  
3️⃣ **Spring Cloud Gateway routes** the request using:  
   🔹 Static routes (YAML-based) or  
   🔹 Dynamic routes (via Eureka discovery)  
4️⃣ **ResponseTraceFilter** adds the correlation ID to the outbound response  
5️⃣ The **response** is sent back to the client 🎯

---

## ⚙️ Gateway Configuration Snippet

### 📄 `application.yml`

```yaml
spring:
  application:
    name: gateway

  cloud:
    gateway:
      discovery:
        locator:
          enabled: true
          lower-case-service-id: true

eureka:
  client:
    registerWithEureka: true
    fetchRegistry: true
    serviceUrl:
      defaultZone: http://localhost:8070/eureka/
```

---

## 🧩 Custom Filters Used

### 1️⃣ **RequestTraceFilter** (Pre-Filter) 📝
🔸 Checks for an existing correlation ID in headers  
🔸 If missing, generates a new UUID and adds it to the request  
🔸 Implements `GlobalFilter` and ordered using `@Order(1)`

### 2️⃣ **ResponseTraceFilter** (Post-Filter) 📤
🔹 Adds the same correlation ID to the response headers  
🔹 Ensures traceability across logs and services

### 3️⃣ **FilterUtility** (Helper) 🧰
🔹 Retrieves and sets headers on `ServerWebExchange`  
🔹 Used by both filters to manipulate HTTP headers

---

## 🗂 Project Structure

```
gateway/
├── filters/
│   ├── RequestTraceFilter.java
│   ├── ResponseTraceFilter.java
│   └── FilterUtility.java
├── application.yml
└── GatewayApplication.java
```

---

## 💡 Real-Time Use Case Example

### Scenario:
A request from frontend hits:
```
GET /accounts/12345
```

🟥 **Without Gateway**:  
⚠️ Client needs to know the actual service host/port  
⚠️ No correlation ID to track logs across services  

🟩 **With Gateway**:  
✅ Request routed based on service name via Eureka  
✅ `travelbank-correlation-id` is added at gateway level  
✅ Downstream services log using same ID

📄 **Log Trace**:
```log
[Gateway] travelbank-correlation-id: abc123
[AccountsService] travelbank-correlation-id: abc123
[Database] travelbank-correlation-id: abc123
```

---

## 🔐 Security & 🔮 Future Plans

🛡 JWT authentication at the gateway level  
🚦 Rate limiting and IP throttling  
📥 Caching for improved performance  
⛑ Circuit breaker integration (Resilience4j)

---

## 🧪 How to Run

1️⃣ Start **Eureka Server (v6.0)**  
2️⃣ Start the **Gateway Service (v7.0)**  
3️⃣ Access any route like:
```
http://localhost:8080/accounts/api/v1/getDetails
```

---

## 📅 Roadmap (v8+)

- 🔑 API Gateway Authentication with Keycloak  
- 🕸 GraphQL Gateway layer  
- 📊 Metrics collection with Prometheus/Grafana  
- 🚀 Canary deployments using Spinnaker or ArgoCD

---

## 🤝 Contribution

✨ Fork, clone, and contribute to shape this modern, cloud-native microservice journey.

---

## 📜 License

MIT License © [Vinay Steja](https://github.com/vinaysteja2)

> Built with 💙 using Spring Boot, Spring Cloud Gateway, WebFlux & Eureka.
