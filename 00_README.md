# ai-oss-gateway

**Status:** Production-Ready | **Tier:** 3 | **Category:** Gateway & API

## Overview

OpenAI-compatible API wrapper

**Domain:** https://0-1.gg/api-oss/ai-oss-gateway  
**Repository:** github.com/0-1-gg/api-oss-fixed  
**License:** Commercial with open governance

---

## Architecture & Components

### Core Components
- router
- auth middleware
- rate limiter
- cache layer

### Specifications

Endpoint: /chat/completions, /embeddings, /models; Protocol: OpenAI API v1; Auth: API Key + Bearer; Rate Limit: 1000 req/min; Latency: <100ms p95; Concurrency: 64+

---

## Deployment Scenarios

### Local Development (docker-compose)
\\\ash
docker-compose up ai-oss-gateway
\\\

### Kubernetes (High Availability)
\\\ash
kubectl apply -f kubernetes-manifests/ai-oss-gateway/
\\\

### Terraform AWS
\\\ash
terraform apply -var="service=ai-oss-gateway"
\\\

---

## Integration Points

See APPENDIX files for detailed integration information:
- 05_PLAYS_WELL_WITH.md — Complementary projects
- 06_System_Integration_Glimpses.md — Real deployment scenarios
- 07_Web_of_Relativity_This_Project.md — Service relationships

---

## Security & Compliance

- **Authentication:** api-oss-security (API Key, OAuth 2.0, JWT)
- **Rate Limiting:** Configurable (default 1000 req/min)
- **Encryption:** TLS 1.3 in transit, AES-256 at rest
- **Audit:** Immutable logging via api-oss-logging
- **Compliance:** HIPAA, GDPR, FedRAMP ready

---

**Last updated:** 2026-09-28
