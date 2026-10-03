# Developer Cookbook — ai-oss-gateway
**Stack:** Python 3.11, FastAPI, JWT (python-jose), rate-limiter, AIOSS_FORMAT
**Domain:** Sovereign AI API gateway: authentication, rate-limiting, routing for Anticloud OSS API
**License:** Apache-2.0 | **IP:** USPTO pending 2026, Anticloud FZ LLE

## Core Usage

```python
from ai_oss_gateway import GatewayApp, JWTConfig

app = GatewayApp(
    jwt_config=JWTConfig(secret_path="./jwt.key", algorithm="HS256"),
    rate_limit=60,  # req/min per client
    aioss_chain="./gateway.aioss"
)

# Issue token
token = app.issue_token(subject="org_001", scopes=["infer", "audit"])

# Route request
resp = await app.route(request, token)
print(resp.target_module, resp.chain_hash)
```

```bash
# Start gateway
python -m ai_oss_gateway --port 8443 --tls-cert ./cert.pem --tls-key ./key.pem
```

```python
# Middleware: add AIOSS chain hash to every response header
@app.middleware("http")
async def aioss_header(request, call_next):
    response = await call_next(request)
    response.headers["X-AIOSS-Chain"] = app.current_chain_hash()
    return response
```

## AIOSS Chain Append

```python
import hashlib, time

def aioss_append(chain_path, payload: bytes, module_id: str):
    entry_hash = hashlib.sha3_256(payload).digest()
    ts = int(time.time_ns()).to_bytes(8, 'big')
    with open(chain_path, 'rb') as f:
        f.seek(-32, 2); prev_hash = f.read(32)
    new_hash = hashlib.sha3_256(prev_hash + entry_hash + ts).digest()
    with open(chain_path, 'ab') as f:
        f.write(ts + entry_hash + new_hash)
    return new_hash.hex()

# After every ai-oss-gateway output:
chain_hash = aioss_append("./ai_oss_gateway.aioss",
                           result_bytes, "ai-oss-gateway")
```

## Performance & Integration

Connection pooling: uvicorn workers = CPU cores. Rate limit per client stored in local Redis alternative (SQLite-backed). JWT validation cached per token TTL. Integration: routes to PAX_API_GATEWAY (T2), authenticated by MF_SO_PASSWORD_MANAGER (T1).
