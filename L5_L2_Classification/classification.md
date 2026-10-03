# L5 Narrow / L2 General Classification — ai-oss-gateway
**Platform:** Anticloud | **PAX:** 27B | **IP:** USPTO pending 2026, Anticloud FZ LLE
**Domain:** Sovereign AI API gateway: authentication, rate-limiting, routing for Anticloud OSS API

## L5 Narrow
ai-oss-gateway specializes in enforcing access control for the Anticloud OSS API surface. It handles JWT issuance, per-client rate limiting, and request routing to downstream TIER_2 PAX modules. Narrow scope: sovereign API traffic only, no cloud proxy behavior.

## L2 General
L2 General: every Anticloud deployment — hospital portal, defense console, robotics dashboard — connects through ai-oss-gateway. One gateway, all deployment contexts.

## PAX Integration
PAX 27B is invoked for intelligent request classification at the gateway: distinguishing clinical queries from robotics queries before routing, without sending data to any external service.

## AIOSS Audit Relevance
Every authenticated API request (client ID + request hash + routing decision + response hash) is AIOSS-chained: H_n = SHA3-256(H_{n-1} || entry_hash_n || timestamp_n).
Tamper-evident, offline-verifiable, zero cloud dependency.

## Regulatory / Compliance
OWASP API Security Top 10, NIST SP 800-95 (web services), OAuth 2.0 / JWT
