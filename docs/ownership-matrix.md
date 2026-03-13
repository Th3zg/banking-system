# Ownership Matrix

Ownership means:
- The service defines the domain model
- The service persists the entity
- The service enforces business rules
- The service is the Source of Truth

---

| Concept | Owner Service | Consistency Model | Notes |
|----------|----------------|------------------|-------|
| User (civil identity) | user-service | Strong (internal) | Personal and regulatory data |
| User business status | user-service | Strong (internal) | Fraud, suspension, compliance |
| Business status projection | auth-service | Strong (internal, replicated) | Minimal replicated state for login validation |
| Authentication identity | auth-service | Strong (internal) | Linked via external_user_id |
| Credentials (password hash) | auth-service | Strong (internal) | Never exposed externally |
| Technical authentication state | auth-service | Strong (internal) | Locking, attempts, 2FA |
| JWT tokens | auth-service | Stateless | Access tokens |
| Refresh tokens | auth-service | Strong (internal) | Persisted and revocable |
| Login attempts | auth-service | Strong (internal) | Security tracking |
