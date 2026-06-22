# Day 21: Background Jobs, Workers, and Scheduled Tasks

## Learning Objectives
- Learn worker patterns for batch processing and async jobs
- Tools: Celery, Sidekiq, Resque, Kubernetes CronJobs
- Retry, idempotency, and visibility timeout handling

## Why This Matters
Background processing offloads long-running work from request paths and improves UX.

---

## Hands-On Exercise
- Implement a job queue with Celery and Redis; schedule periodic tasks and retries.

## Mini Project
- Build a thumbnail generation pipeline: upload -> job -> processed asset stored in CDN.

## Expected Outcome
You can design and implement robust background job systems with retries and idempotency.
