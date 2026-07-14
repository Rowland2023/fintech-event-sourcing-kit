# fintech-domain-kit

Production-ready domain building blocks for payment platforms, wallets, escrow systems, marketplaces, and financial ledgers.

Built on top of Domain-Driven Design principles to help teams build audit-safe financial software.

Designed for correctness before convenience.

---

## Why this exists

Financial systems require more than CRUD.

They require:

- auditability
- consistency
- concurrency protection
- immutable history
- idempotency
- reliable event delivery

This library packages reusable patterns commonly required by regulated payment systems.

---

## Solves Problems Like

- Duplicate payment processing
- Double escrow release
- Lost webhook events
- Missing audit history
- Race conditions
- Concurrent updates
- Event publication failures
- Inconsistent payment state

---

## Features

- Payment Aggregates
- Ledger Building Blocks
- Domain Events
- Money Value Objects
- Optimistic Locking
- Transactional Outbox
- Idempotency Patterns
- Correlation & Causation IDs
- Immutable Audit Events
- Payment State Machines

---

## Core Components

### Payment Aggregate

Responsible for enforcing payment lifecycle invariants.

Example:

```javascript
payment.markSuccessful({

    gatewayTransactionId,

    paidAt,

    correlationId

});
```

---

### Money

Immutable value object.

Supports:

- amount validation
- currency validation
- arithmetic safety

---

### Domain Events

Examples include:

- PaymentCreated
- PaymentAuthorized
- PaymentCaptured
- PaymentReleased
- PaymentRefunded
- PaymentFailed

Each event contains:

- aggregateId
- occurredAt
- correlationId
- causationId
- immutable payload

---

### Optimistic Concurrency

Every aggregate maintains a version.

Repositories perform updates similar to:

```sql
UPDATE payments
SET version = version + 1
WHERE id = ?
AND version = ?
```

Concurrent modifications fail safely.

---

### Transactional Outbox

Aggregate state and domain events are committed atomically.

```
Payment Updated
        +
Domain Events
        +
Outbox Row

        │

     COMMIT

        │

 Background Publisher

        │

      Kafka
```

No dual-write failures.

---

### Correlation IDs

Every business action can be traced across services.

```
Webhook

↓

Payment

↓

Ledger

↓

Notification

↓

Email
```

Single correlation ID.

Complete audit trail.

---

## Example

```javascript
payment.release({

    correlationId,

    releasedAt

});

await unitOfWork.execute(async () => {

    await paymentRepository.save(payment);

    await outbox.store(
        payment.pullEvents()
    );

});
```

---

## Suitable For

- Digital Wallets
- Payment Gateways
- Escrow Platforms
- Marketplaces
- Banking Systems
- Lending Platforms
- Accounting Systems
- Subscription Billing
- Fintech APIs

---

## Design Principles

- Domain-Driven Design
- Financial Correctness
- Auditability
- Immutable Events
- Event-Driven Architecture
- Optimistic Locking
- Transactional Outbox
- Explicit Business Invariants

---

## Built From

This project was extracted from a modular payment architecture developed as part of a larger conference management platform.

The same patterns are used across multiple bounded contexts including:

- Payments
- Notifications
- Registrations
- Conferences

The goal is to provide reusable financial domain components independent of any specific framework or database.

---

## Roadmap

- Payment Aggregate
- Refund Aggregate
- Ledger Aggregate
- Wallet Aggregate
- Escrow Aggregate
- Split Payment Support
- Double-Entry Ledger Utilities
- Saga Examples
- Kafka Integration Samples

---

## License

MIT
