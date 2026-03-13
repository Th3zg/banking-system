# Global Architecture

## 1. Architectural Style

The system follows a hybrid microservices architecture:

- Event-driven architecture as the backbone.
- REST/gRPC for synchronous communication.
- Database per service.
- Explicit bounded contexts.
- Clear Source of Truth definition per domain.
---

## 2. Communication Strategy

### 2.1 Asynchronous Communication

Apache Kafka is used for domain event propagation.

Each service publishes events related to its own domain.
Other services consume events to react accordingly.

The Outbox pattern is used to guarantee atomicity
between database persistence and event publication.

Asynchronous communication is used for:
- State propagation
- Notifications
- Audit
- Analytics
- Non-critical reactions

---

### 2.2 Synchronous Communication

REST is used for:

- External API exposure via API Gateway.
- Administrative or immediate consistency operations.
- Cross-service coordination of critical state transitions.

gRPC may be introduced for internal low-latency service-to-service calls.
Security-critical operations MUST NOT depend on eventual consistency.

---

## 3. Data Management

- Each service owns its database schema.
- No cross-database queries are allowed.
- Cross-service data synchronization occurs via events or controlled REST coordination.
- No service queries another service's database.

---

## 4. Consistency Model

The system embraces eventual consistency for cross-service propagation.

Strong consistency is required:

- Within service boundaries.
- For authentication decisions.
- For fraud-related state transitions.

Authentication decisions must not depend on eventual consistency.

---

## 5. Security Architecture

- JWT-based authentication (stateless access tokens).
- Persisted refresh tokens (revocable).
- Role-based authorization.
- Optional 2FA.
- Technical lock policies (auth-service).
- Business lock policies (user-service).
- Replicated minimal business status in auth-service.

---

## 6. Critical State Coordination

When a business-level fraud block occurs:

1. `user-service` updates its internal state.
2. `user-service` performs a synchronous REST call to `auth-service`.
3. `auth-service` updates its internal projection of business status.
4. Both services publish domain events for audit propagation.

This guarantees zero inconsistency window for login validation.

---

## 7. Future Architectural Evolution

- Saga pattern for distributed financial transactions.
- Dedicated audit service consuming all domain events.
- Observability (centralized logging, tracing, metrics).
- Risk-engine microservice.
