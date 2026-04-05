# ACIS — Autonomous AI Cost Intelligence System

> **"The Stripe for AI usage optimization."**

ACIS intelligently routes every AI prompt to the optimal model — balancing **cost × quality × speed** — so you never overpay for AI again.

---

## What Makes ACIS Different

| Feature | ACIS | LiteLLM | OpenRouter | Helicone |
|---|---|---|---|---|
| **Simulate Before Execute** | ✅ Preview cost + quality before spending | ❌ | ❌ | ❌ |
| **Auto-Cascading Fallback** | ✅ Try cheap → escalate if quality fails | Partial | ❌ | ❌ |
| **Self-Learning Routing** | ✅ Gets smarter with every query | ❌ | ❌ | ❌ |
| **Cost Savings Dashboard** | ✅ Real-time $ saved counter | Basic | Basic | ✅ |
| **Task Classification** | ✅ Auto-detects task type + complexity | ❌ | ❌ | ❌ |
| **Quality Evaluation** | ✅ LLM-as-judge on every response | ❌ | ❌ | ❌ |

---

## Quick Start

### 1. Clone & Configure
```bash
git clone <your-repo-url>
cd acis
cp .env.example .env
# Add your API keys to .env
```

### 2. Run with Docker
```bash
docker compose up -d
```

### 3. Or run locally
```bash
pip install -e .
uvicorn main:app --reload
```

### 4. Open
- API Docs: http://localhost:8000/docs
- Dashboard: Open the React dashboard (see `/dashboard`)

---

## API Usage

### Send a Query (Optimized Routing)
```bash
curl -X POST http://localhost:8000/api/v1/query \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "Write a Python function to merge two sorted lists",
    "latency_priority": "medium"
  }'
```

### Simulate Before Execute (USP)
```bash
curl -X POST http://localhost:8000/api/v1/simulate \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "Analyze the competitive landscape of AI infrastructure startups",
    "simulate_only": true
  }'
```

### Get Cost Savings Report
```bash
curl http://localhost:8000/api/v1/analytics/savings?hours=24
```

---

## Architecture

```
Request → Cache Check → Task Classifier → Routing Engine → Provider
                                              ↓
                                         Cost Engine
                                              ↓
                                    Evaluation Engine
                                              ↓
                                   Quality < threshold?
                                     ↓ YES         ↓ NO
                                  Cascade        Return
                                  to better      response
                                  model
                                              ↓
                                    Memory + Learning
                                              ↓
                                    Dashboard Analytics
```

---

## Project Structure

```
acis/
├── main.py                    # FastAPI entry point
├── config/
│   └── settings.py            # Environment configuration
├── app/
│   ├── api/
│   │   └── routes.py          # API endpoints
│   ├── core/
│   │   ├── classifier.py      # Task classification
│   │   ├── model_registry.py  # Model catalog + pricing
│   │   ├── routing_engine.py  # Smart routing + cascading
│   │   ├── cost_engine.py     # Cost estimation + simulation
│   │   ├── evaluation.py      # LLM-as-judge quality scoring
│   │   ├── cache.py           # Query caching
│   │   └── memory.py          # Analytics + learning store
│   ├── integrations/
│   │   └── providers.py       # OpenAI / Anthropic / Google wrappers
│   └── services/
│       └── orchestrator.py    # Main execution pipeline
├── docker-compose.yml
├── Dockerfile
└── pyproject.toml
```

---

## Self-Learning Loop

ACIS gets smarter with every query:

1. **Record**: Every query's model, cost, latency, and quality score
2. **Analyze**: After 5+ data points per model, historical averages influence routing
3. **Adjust**: Routing preferences shift toward models that perform best per task type
4. **Report**: `/analytics/routing-preferences` shows learned optimal models

---

## Deployment

### Railway
```bash
railway init
railway link
railway up
```

### Fly.io
```bash
fly launch
fly deploy
```

---

## License

MIT
