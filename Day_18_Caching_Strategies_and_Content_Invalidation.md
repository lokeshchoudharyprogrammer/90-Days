# Day 18: Caching Strategies and Content Invalidation

## Learning Objectives
- Learn caching layers: client, CDN, reverse-proxy, in-memory, distributed cache
- Cache invalidation strategies and fallbacks (TTL, write-through, write-back)
- Understand cache stampede and mitigation patterns

## Why This Matters
Caching reduces latency and load but requires careful invalidation to avoid stale data.

---

## Hands-On Exercise
- Implement Redis as a cache for database queries with TTL and compare response times.

## Mini Project
- Design a cache strategy for a read-heavy product catalog with per-item TTL and invalidation hooks.

## Expected Outcome
You can design caching layers, choose policies, and implement safe invalidation.
