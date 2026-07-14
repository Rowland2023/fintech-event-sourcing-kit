# Fintech-Event-Kit

Production-ready base classes for audit-safe payment systems. Used in escrow, ledgers, split payments.

## Why this exists
Prevents the $50k bugs: double-spend, missing audit trail, webhook race conditions.

## Core patterns
1. `Entity` - Optimistic locking via `version` + `incrementVersion()`
2. `DomainEvent` - Immutable events with `correlationId`, `causationId`, `occurredAt`
3. `Aggregate` - Emits events, no direct DB writes

## Example: Preventing Double-Release
See `src/aggregates/Payment.js` - `release()` method uses version check.

## Used by
Built after 6 hours of unpaid "Excellent" interviews with talent platforms.
I ship code, not free consulting.

## Hire me
If your payment system can't prove "this wasn't paid twice", DM me on LinkedIn.
I do 1-hour paid audits.
