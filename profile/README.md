# Mobility Systems

A production-grade microservices platform exploring modern distributed-systems
patterns: **OAuth 2.1 + OIDC**, **gRPC** for internal calls, **RabbitMQ** for
async workflows, and the **saga pattern** for distributed transactions with
compensating rollbacks across service boundaries.

**Built with Java 21 · Spring Boot 3.5 · Spring Authorization Server · Go · PostgreSQL · Redis · RabbitMQ**

```mermaid
flowchart LR
    Client([Client]) -->|OAuth 2.1 + PKCE| Auth
    Client -->|REST + JWT| AM[Account Management]
    Client -->|REST + JWT| RC[Racing Core]
    AM -->|gRPC| Auth[Auth Server]
    RC -->|gRPC| Auth
    RC -->|REST| AM
    AM -->|AMQP| MQ{{RabbitMQ}}
    RC -->|AMQP| MQ
    MQ -->|rollback events| Auth
    MQ -->|email events| Email[Email Service]
```

## The platform

Six repositories, each owning a clear responsibility:

- **[auth-server](https://github.com/mobility-systems/auth-server)** — OAuth 2.1 + OIDC authorization server built on Spring Authorization Server
- **[account-management](https://github.com/mobility-systems/account-management)** — user & organization registration (Spring Boot, saga orchestrator)
- **[racing-core](https://github.com/mobility-systems/racing-core)** — domain service for cars, drivers, laps, tracks (Spring Boot, saga orchestrator)
- **[email-service](https://github.com/mobility-systems/email-service)** — Go-based RabbitMQ consumer for transactional email
- **[mobility-common](https://github.com/mobility-systems/mobility-common)** — shared Maven library (gRPC protos, RabbitMQ queue definitions, saga primitives)
- **[mobility-app](https://github.com/mobility-systems/mobility-app)** — umbrella repo with architecture docs, ADRs, and `docker-compose.yml`

## 👉 Start here: [mobility-app](https://github.com/mobility-systems/mobility-app)

The umbrella repo contains the architecture deep dive, saga flow diagrams,
architectural decision records, and instructions to run the full platform locally.

---

*Source-available portfolio project. See each repo's NOTICE for usage terms.*