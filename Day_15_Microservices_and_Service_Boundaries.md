# Day 15: Microservices Architecture & Service Boundaries

## Learning Objectives
- Understand microservices principles, pros/cons vs monoliths
- Identify service boundaries and data ownership
- Learn service communication patterns and anti-patterns

## Why This Matters
Microservices enable independent deployability and scaling but introduce operational complexity. Correct boundaries minimize coupling and coordination costs.

---

## Beginner → Senior Progression
- Beginner: split app into small services by feature
- Intermediate: design APIs, versioning, and backward compatibility
- Advanced: handle distributed transactions, sagas, and cross-service testing

---

## Core Concepts
- Bounded context, data ownership, API contracts, versioning, lateral scaling

## Hands-On Exercise
- Break a monolithic TODO app into two services (users and tasks) with HTTP APIs and test interactions.

## Mini Project
- Design and implement two microservices with clear contracts, include tests and simple deployment scripts.

## Expected Outcome
You can design service boundaries and implement small microservices communicating over HTTP or messaging.
