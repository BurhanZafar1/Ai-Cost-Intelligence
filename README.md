# ACIS — AI Cost Intelligence System

> **"The Stripe for AI usage optimization."**

Intelligently route every AI prompt to the optimal model — balancing **cost × quality × speed** — so you never overpay for AI again.

## ✨ Live Features

- **Simulate Before Execute** — Preview cost + quality across 10 models before spending a token
- **Auto-Cascading Fallback** — Try cheap model → escalate automatically if quality is low
- **Self-Learning Routing** — Gets smarter with every query
- **Cost Savings Dashboard** — Real-time savings counter with charts
- **Task Classification** — Auto-detects code / creative / analysis / support tasks

---

## 🚀 Deploy in 5 Minutes (No Terminal Needed)

### Step 1 — Get the code on GitHub

1. Go to [github.com/new](https://github.com/new)
2. Name your repo `acis` (or anything you like)
3. Set it to **Public**
4. Check **"Add a README file"**
5. Click **Create repository**
6. On your new repo page, click **"Add file"** → **"Upload files"**
7. Drag and drop ALL files from the downloaded zip into the upload area
8. Click **"Commit changes"**

> **Important:** Make sure the files are at the ROOT of the repo, not inside a subfolder. You should see `package.json`, `index.html`, `vite.config.js` etc. at the top level.

---

### Step 2 — Deploy Frontend on Vercel (FREE)

1. Go to [vercel.com](https://vercel.com) and click **"Sign Up"** → **"Continue with GitHub"**
2. Authorize Vercel to access your GitHub
3. Click **"Add New..."** → **"Project"**
4. Find your `acis` repo in the list and click **"Import"**
5. Vercel auto-detects Vite. Leave all settings as default:
   - Framework Preset: **Vite**
   - Build Command: `npm run build`  
   - Output Directory: `dist`
6. Click **"Deploy"**
7. Wait ~60 seconds. You'll get a live URL like: **`https://acis-yourusername.vercel.app`**

✅ **Done! Your dashboard is live.** The simulation mode works immediately — no backend needed.

---

### Step 3 (Optional) — Deploy Backend on Render (FREE)

This gives you a live API with real LLM routing.

1. Go to [render.com](https://render.com) and sign up with GitHub
2. Click **"New +"** → **"Web Service"**
3. Connect your `acis` GitHub repo
4. Configure:
   - **Name:** `acis-backend`
   - **Root Directory:** `backend`
   - **Runtime:** Python
   - **Build Command:** `pip install -r requirements.txt`
   - **Start Command:** `uvicorn main:app --host 0.0.0.0 --port $PORT`
5. Under **Environment Variables**, add your API keys:
   - `OPENAI_API_KEY` = your key
   - `ANTHROPIC_API_KEY` = your key  
   - `GOOGLE_API_KEY` = your key
6. Select the **Free** plan
7. Click **"Create Web Service"**
8. Wait ~2 minutes for build. You'll get a URL like: **`https://acis-backend.onrender.com`**
9. Test it: visit `https://acis-backend.onrender.com/docs` to see the API docs

---

## 📁 Project Structure

```
acis/
├── index.html              ← Frontend entry point
├── package.json            ← Frontend dependencies
├── vite.config.js          ← Vite build config
├── vercel.json             ← Vercel routing config
├── src/
│   ├── main.jsx            ← React entry
│   └── App.jsx             ← Full dashboard app
├── backend/
│   ├── main.py             ← FastAPI server
│   ├── requirements.txt    ← Python dependencies
│   ├── .env.example        ← Environment template
│   ├── config/
│   │   └── settings.py     ← Configuration
│   ├── app/
│   │   ├── api/routes.py        ← API endpoints
│   │   ├── core/
│   │   │   ├── classifier.py    ← Task classification
│   │   │   ├── model_registry.py ← 10 models with pricing
│   │   │   ├── routing_engine.py ← Smart routing + cascade
│   │   │   ├── cost_engine.py   ← Cost simulation
│   │   │   ├── evaluation.py    ← Quality scoring
│   │   │   ├── cache.py         ← Response caching
│   │   │   └── memory.py        ← Analytics + learning
│   │   ├── integrations/
│   │   │   └── providers.py     ← OpenAI/Anthropic/Google
│   │   └── services/
│   │       └── orchestrator.py  ← Execution pipeline
│   └── tests/
│       └── test_acis.py         ← 40 passing tests
└── render.yaml             ← Render deploy config
```

---

## 🧪 API Endpoints (Backend)

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/v1/query` | Send prompt → get optimized response |
| POST | `/api/v1/simulate` | Preview cost across all models (no spend) |
| POST | `/api/v1/classify` | Classify a prompt's task type |
| GET | `/api/v1/models` | List all models with pricing |
| GET | `/api/v1/analytics/savings` | Cost savings report |
| GET | `/api/v1/analytics/logs` | Recent query history |
| GET | `/api/v1/health` | System health check |

---

## License

MIT
