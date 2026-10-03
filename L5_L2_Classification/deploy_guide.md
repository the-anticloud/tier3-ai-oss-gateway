# Deploy Guide — ai-oss-gateway
**Platform:** Anticloud sovereign infrastructure | Air-gap capable
**Stack:** Python 3.11, FastAPI, JWT (python-jose), rate-limiter, AIOSS_FORMAT

## Prerequisites
Python 3.11+, FastAPI 0.110+, python-jose 3.3+, slowapi (rate limiting), uvicorn

## AIOSS Integration
```bash
aioss init --module ai-oss-gateway --output ./ai_oss_gateway.aioss
aioss append --chain ./ai_oss_gateway.aioss --payload ./output.bin --module ai-oss-gateway
aioss verify --chain ./ai_oss_gateway.aioss
```

## Air-Gap Deployment
```bash
# On networked machine:
pip download -r requirements.txt -d ./wheels/
# Transfer wheels/ to air-gap host, then:
pip install --no-index --find-links ./wheels/ -r requirements.txt
```

## PAX 27B Harness Wiring
```python
from anticloud_pax import PAXHarness
harness = PAXHarness(
    model_path="./pax-27b-q4.gguf",
    module="ai-oss-gateway",
    aioss_chain="./ai_oss_gateway.aioss",
    classification="L5_NARROW_L2_GENERAL"
)
result = harness.process(input_data)
```

## Verification
```bash
aioss verify --chain ./ai_oss_gateway.aioss --verbose
python -m ai_oss_gateway.tests.smoke
```
