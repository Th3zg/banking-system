# Bank Project — System Overview

## 1. Objective

Digital banking system (neobank) built on a microservices architecture.  
The goal is to model a modern banking core with:

- Strict domain separation
- Event-based communication
- Eventual consistency
- Security as a first-class concern
- Horizontal scalability

This project is intended for professional portfolio purposes and architectural experimentation.

---

## 2. Architectural Principles

- Each microservice owns its own database.
- No cross-database access is allowed.
- Synchronous communication via REST or gRPC.
- Asynchronous communication via Kafka.
- Event-driven architecture as the predominant model.
- CQRS applied where appropriate.
- Source of Truth clearly defined per service.

---

## 3. Current Microservices

### user-service
Responsible for:
- User management
- Civil/personal information
- User status
- Relationship with future accounts

It is the Source of Truth for the user as a business entity.

---

### auth-service
Responsible for:
- Authentication
- Authorization
- Credential management
- JWT and refresh token handling
- Technical account locking
- 2FA management

It is the Source of Truth for the authenticable identity.

---

## 4. Communication

- `user-service` publishes events such as `UserCreated`.
- `auth-service` consumes relevant events to create an authenticable identity.
- Services do not share databases.
- Internal communication may use gRPC in the future.
- External communication will be exposed via an API Gateway.

---

## 5. Current Project Status

The system is currently in an architectural consolidation phase,  
with `user-service` and `auth-service` partially implemented.
